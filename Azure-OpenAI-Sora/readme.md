# How to use the Sora API in Azure OpenAI Service (Azure AI Foundry)
![sora](https://i.imgur.com/hDMSTZR.png)

OpenAI's Sora model is a cutting-edge text-to-video AI that transforms written prompts into vivid, high-quality video clips. Now available through Azure OpenAI, Sora empowers developers and creators to generate up to 20-second videos in 1080p resolution directly from text, making it ideal for industries like advertising, media, and enterprise training. With built-in safety features and seamless integration into Azure’s ecosystem, users can deploy Sora via the Azure AI Foundry and even scale development using tools like Visual Studio Code. 

In this article I will describe how to use the Azure OpenAI Sora API to generate your own videos!

## Prereq: Create a Sora Deployment
Before you do anything, you have to spin up your own deployment of the Sora model in your own Azure OpenAI (in Azure AI Foundry) project. [This article](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/create-resource?pivots=web-portal) walks you through that process. This is important as every subsequent request will be against *your* unique Azure OpenAI service (replace my `https://ai-myproject.openai.azure.com` with your deployment's unique endpoint).

After creating a Sora deployment in your Azure OpenAI project, Azure AI Foundry will very neatly show you the target URL you can call to for any subsequent API call as well as the API key you will use as a header, shown in examples below.

![sora deployed](https://i.imgur.com/O08FVoh.png)

## Step 1: Request Video Generation
The first step in the process is to perform a POST request to the Azure OpenAI service to *request* video generation. Example below:

```
POST https://ai-myproject.openai.azure.com/openai/v1/video/generations/jobs?api-version=preview
Content-Type application/json
Api-key 12abd080a3734bc2acdde9c3c71afbb3

{
  "model": "my-sora-model",
  "prompt": "A rocket landing on mars",
  "height": 1080,
  "width": 1080,
  "n_seconds": "5",
  "n_variants": "1"
}
```

The above POST call requests video generation. A successful response will look like this:

```
201 Created
Content-Type application/json

{
  "object": "video.generation.job",
  "id": "task_01jy98zkvpfb7909522ysd150a",
  "status": "queued",
  "created_at": 1750510587,
  "finished_at": null,
  "expires_at": null,
  "generations": [],
  "prompt": "A rocket landing on mars",
  "model": "my-sora-model",
  "n_variants": 1,
  "n_seconds": 5,
  "height": 1080,
  "width": 1080,
  "failure_reason": null
}
```

## Step 2: Poll Status
As seen above, unlike the traditional large language models you have already interfaced with in Azure OpenAI, the Sora API *does not* immediately return the resulting video. Instead, it confirms it has received your request successfully and is now working on it.

We can then perform another API call, this time a GET request, to check the status of our video generation! The `id` property identifies the video generation job the Sora API has created and assigned to our video generation task. We can take that `id` property and append it to a URL as shown below to check its status:

```
GET https://ai-myproject.openai.azure.com/openai/v1/video/generations/jobs/task_01jy98zkvpfb7909522ysd150a?api-version=preview
Api-key 12abd080a3734bc2acdde9c3c71afbb3
```

The above request will return a response like this while the video is being generated:

```
200 OK
Content-Type application/json

{
  "object": "video.generation.job",
  "id": "task_01jy98zkvpfb7909522ysd150a",
  "status": "running",
  "created_at": 1750510587,
  "finished_at": null,
  "expires_at": null,
  "generations": [],
  "prompt": "A rocket landing on mars",
  "model": "my-sora-model",
  "n_variants": 1,
  "n_seconds": 5,
  "height": 1080,
  "width": 1080,
  "failure_reason": null
}
```

And once the video is done being generated (if we wait 30 seconds later), the response will indicate the generation has succeeded under the `status` property:

```
200 OK
Content-Type application/json

{
  "object": "video.generation.job",
  "id": "task_01jy98zkvpfb7909522ysd150a",
  "status": "succeeded",
  "created_at": 1750510587,
  "finished_at": 1750510621,
  "expires_at": 1750597021,
  "generations": [
    {
      "object": "video.generation",
      "id": "gen_01jy990gaxeayb74aq4hnbn2cp",
      "job_id": "task_01jy98zkvpfb7909522ysd150a",
      "created_at": 1750510619,
      "width": 1080,
      "height": 1080,
      "n_seconds": 5,
      "prompt": "A rocket landing on mars"
    }
  ],
  "prompt": "A rocket landing on mars",
  "model": "my-sora-model",
  "n_variants": 1,
  "n_seconds": 5,
  "height": 1080,
  "width": 1080,
  "failure_reason": null
}
```

## Step 3: Retrieve the Video
Finally, we can perform the last step in the video generation process: retrieve the video! In the `status: succeeded` response from step 2, will see it provides another `id` of the generation directly (**not** the job ID). In my case, it is `gen_01jy990gaxeayb74aq4hnbn2cp`. We can use this ID in *another* GET request to retrieve the completed video, shown below:

```
GET https://ai-myproject.openai.azure.com/openai/v1/video/generations/gen_01jy990gaxeayb74aq4hnbn2cp/content/video?api-version=preview
Api-key 12abd080a3734bc2acdde9c3c71afbb3
```

And the response we get will be the video content directly!

![rocket](https://i.imgur.com/EUIfct0.png)

## Close
With these steps, you're now equipped to bring your creative vision to life using the Sora API within Azure OpenAI. Whether you're prototyping marketing content, simulating training scenarios, or just exploring the possibilities of AI-generated video, this powerful toolset allows you to go from idea to immersive experience with just a few lines of code. Happy building!