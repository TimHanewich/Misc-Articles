# Building and Invoking AI Agents in Azure AI Foundry's Agent Service via API
![banner](https://i.imgur.com/IMS6AD6.png)

The Azure AI Foundry Agent Service is a fully managed platform from Microsoft designed to help developers build, deploy, and scale intelligent AI agents for enterprise use. It's part of the broader Azure AI Foundry ecosystem, which acts like a digital factory for creating AI-powered applications and agents.

The Azure AI Foundry Agent Service offers all the capabilities needed to build powerful, reliable, and intelligent AI agents for enterprise use:
- **Model Integration**: Supports state-of-the-art language models like GPT-4o, Llama, and Mistral
- **Tool Access**: Agents can interact with enterprise data through Bing Search, SharePoint, Azure Logic Apps, and other services
- **Multi-Agent Collaboration**: Enables agents to work together, delegate tasks, and share context across workflows
- **Observability**: Provides performance insights and debugging support through Application Insights
- **Security & Governance**: Built on Microsoft Entra with encryption and role-based access control for enterprise-grade security

In this article I'll share how you can create your own agent in Azure AI Foundry Agent Service and begin interacting with it via REST API!

## But first: What is an Agent?
You can think of an AI Agent as *a configuration of resources* surrounding an underlying large language model (LLM). 

![agent](https://i.imgur.com/isWp7GZ.png)

As depicted above, there are several resources we may provide to an agent that allow the underlying LLM be more successful in accomplishing the task it has been asked to do. These may include:
- **Instructions**: we provide the model with instructions on how to go about doing its job.
- **History**: we provide the model with the chat history from a conversation thread with a user so the model knows what was previously being discussed earlier in the conversation.
- **Settings**: some settings, like `temperature` or `top_p` slightly alter the way the model behaves.
- **Knowledge**: data (PDFs, word documents, website content, databases, knowledge articles, etc.) that we provivde to the model that it can use to find relevant answers in.
- **Tools** (a.k.a. "functions"): discrete capabilities the model can call upon to take action in a system in its journey of satisfying the user's request to it.

Services like **Azure AI Foundry Agent Service** and **Copilot Studio**, both by Microsoft, allow us to have an easy-to-use infrastructure for setting up and using these agents!

## Step 1: Create the Agent in the Portal
The very step to using the Azure AI Foundry Agent Service is to create *an Agent*.

There are two ways to do this: 1) via the Azure AI Foundry portal (website with a GUI), or 2) programatically via an API call. For the sake of this article, I'll show it visually in the portal:

This assumes you already have a **project** established in Azure AI Foundry. If you haven't already, see [this documentation](https://learn.microsoft.com/en-us/azure/ai-foundry/how-to/create-projects?tabs=ai-foundry&pivots=hub-project).

Navigate to the Azure AI Foundry portal: [ai.azure.com](https://ai.azure.com).

![Agent Service Portal](https://i.imgur.com/jOUqFQd.png)

On the left, select "Agents". This will present you with a list of agents that you have made so far within the project you are currently within.

At the top, click on "+ New Agent" to create an agent. And that is it! Just like that, you have created a new agent. In the pane on the right is where you can further customize your agent, meaning giving it various resources it can use at its dispossal to accomplish any task you, or a user, give it.

As you can see in the screenshot above, creating the agent does require you to select a **Deployment**. This is where you would select what underlying large language model (LLM) you want this agent to use for its inference (generating its response). If you aren't seeing one in the list there, you just need to deploy an instance of your own! Click "+ Create new deployment" and select a model you like - I'm using GPT-4.1-mini and it works quite well for most basic tasks.

Normally, we would add additional resources to the agent: elaborate instructions (I added basic instructions), knowledge sources, actions, connected agents, and more. For the sake of this article, I'll skip that for now to keep things simple, but feel free to explore with adding those if you wish.

## Step 2: Get a Bearer Token
Unlike with Microsoft's **Copilot Studio**, which provides a graphic interface for us to interface with our agent, Azure AI Foundry Agent Service serves purely as a middle layer between a user interface (i.e. a chatbox) and an underlying LLM that **we call to via API**.

Before we do *anything* with our agent via API, we first must first authenticate. To do this, we will use [the Azure CLI](https://learn.microsoft.com/en-us/cli/azure/what-is-azure-cli?view=azure-cli-latest) to get a [bearer token](https://swagger.io/docs/specification/v3_0/authentication/bearer-authentication/) that we can provide in all subsequent API calls; Azure AI Foundry will use this to confirm we are who we say we are and thus have permission to interact with it!

Before proceeding, you will need to download and install the Azure CLI. You can find instructions on how to do that on [the Azure CLI Docs page](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

With the Azure CLI installed, open command prompt and enter:
```
az login
```

This will prompt you to log in to your tenant in which your agent lives.

After logging in, run the following to be granted a bearer token.

```
az account get-access-token --scope "https://ai.azure.com/.default"
```

(note, I know [the official documentation](https://learn.microsoft.com/en-us/azure/ai-foundry/agents/quickstart?pivots=rest-api#api-call-information) calls for us to run `az account get-access-token --resource 'https://ai.azure.com'`, but I've never had success with that. Using what I provided above instead has worked.)

Once successful, the Azure CLI should reply with a bearer token in a payload that looks similar to below:

```
{
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Il9qTndqZVNudlRUSzhYRWRyNVFVUGtCUkxMbyIsImtpZCI6Il9qTndqZVNudlRUSzhYRWRyNVFVUGtCUkxMbyJ9.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.EZ99HaCBva1Wui2XEmu5bVZHzmG752JH6Iinkl0QQKOV6n-_4RR4MEZRz34WISh7QnOiF5MElrrA9vms2gMxyo8pTgw-T7Jq5c7wrgzoVxerOOw9yCINBKSPaF63We2qWTlAGKO4WgyCjN46js5beQk6RQth7awT60e7hK2ZD02y232RM-_qOoIv9iyti6H-3roATqpnstL8CzgX850b7A4LV7ZijGLsQlx6_XaZhr98DVdjNXZDP7VaPRMOd77R1ob0xUmX8U4xnK9fQyHsnDslYmuhppkIZydiuPI388YYkBaArl_xNwNocmsGC1tOp4-0Lxwo32iJGPvXAm7XVQ",
  "expiresOn": "2025-07-15 10:45:37.000000",
  "expires_on": 1752590737,
  "subscription": "f6ee3d28-3d48-4a04-a3cc-a88f6553d7ad",
  "tenant": "16b3c013-d300-468d-ac64-7eda0820b6d3",
  "tokenType": "Bearer"
}
```

Specifically, the `accessToken` property ("eyJ0eXAiOiJKV1QiLCJhb...") is what we want! Copy that entire long string as that will come in handy next.

This token is valid for only a period of time - perhaps an hour or so - and we can use it to authenticate the next API calls...

## Step 3: Create a Thread
Now that we can begin calling the Azure AI Foundry Agent Service, the very first step is to **create a thread**.

You can think of a thread as chat conversation history; it is literally just a list of all messages exchanged between an AI and a user. So, before we begin a conversation, we must create a container for all conversation history; this is called "a thread".

To create a thread, we can perform a `POST` call to our Azure AI Foundry Agent Service project deployment. 

Don't know what you project deployment endpoint is? You can find it on the **Overview** page in the Azure AI Foundry portal!

![endpoint](https://i.imgur.com/MB1n16W.png)

The raw endpoint will look something like this (`TestAgents` is the name of my project): `https://testagents-resource.services.ai.azure.com/api/projects/TestAgents`

We can append `/threads?api-version=2025-05-01` to the end to access the endpoint that will allow us to make a thread:

```
POST https://testagents-resource.services.ai.azure.com/api/projects/TestAgents/threads?api-version=2025-05-01
Authorization: Bearer <<YOUR_ACCESS_TOKEN>>
```

*in the call above, replace `<<YOUR_ACCESS_TOKEN>>` with your actual bearer token we got in step 1*


The API will respond do you with something like below, confirming the thread has been made by providing you with the thread ID (`thread_L9HRkMrJcHy7ZOiR9I4J6ckY` in this case).

```
{
  "id": "thread_L9HRkMrJcHy7ZOiR9I4J6ckY",
  "object": "thread",
  "created_at": 1752587659,
  "metadata": {},
  "tool_resources": {}
}
```

## Step 4: Add a User Message
Now that we have our thread created, we are ready to load it up with a message! Let's initiate the conversation with the AI...

The example below loads a question from the user, "Why is the sky blue?" into the thread. Note in the endpoint we are posting to, we are directly adding this message to the thread we previously created by placing the thread ID in the endpoint.

```
POST https://testagents-resource.services.ai.azure.com/api/projects/TestAgents/threads/thread_L9HRkMrJcHy7ZOiR9I4J6ckY/messages?api-version=2025-05-01
Authorization: Bearer <<YOUR_ACCESS_TOKEN>>
Content-Type: application/json

{
    "role":"user", 
    "content": "Why is the sky blue?"
}
```

For those of you that may have used the OpenAI API service, Azure OpenAI service, Ollama, or any other LLM inference service before, the format above will look very familiar.

To confirm the message has been added to the thread, the Azure AI Foundry Agent Service API will respond with the following:

```
{
  "id": "msg_zE1gyAZnopYj2SQ5zn8vTyWr",
  "object": "thread.message",
  "created_at": 1752587787,
  "assistant_id": null,
  "thread_id": "thread_L9HRkMrJcHy7ZOiR9I4J6ckY",
  "run_id": null,
  "role": "user",
  "content": [
    {
      "type": "text",
      "text": {
        "value": "Why is the sky blue?",
        "annotations": []
      }
    }
  ],
  "attachments": [],
  "metadata": {}
}
```

## Step 5: Run a Thread
Now, unlike with Azure OpenAI service for example, we have *not* invoked inference (the generation of the response) yet. We have only added the user's message to the thread. It will sit in the thread and wait there until we invoke inference, a.k.a. *run the thread*.

*This* is the stage in which we are going to specifically invoke the agent we created back in step 1. As seen in the example below, we make a POST request to the thread we have been working in so far, with `/runs` appended at the end. This will trigger inference to begin, during which the message we added in the thread will be processed and answered.

```
POST https://testagents-resource.services.ai.azure.com/api/projects/TestAgents/threads/thread_L9HRkMrJcHy7ZOiR9I4J6ckY/runs?api-version=2025-05-01
Authorization: Bearer <<YOUR_ACCESS_TOKEN>>
Content-Type: application/json

{
    "assistant_id": "asst_SUiBkwY2MsknWcBfWo5tfEtc"
}
```

As seen above, we specify which **agent** should be used to generate the response (inference) by specifying `assistant_id` in the body. We are providing the ID of the agent that we created back in step 1 (it was clearly noted in the portal).

Again, unlike with Azure OpenAI servie or other LLM inference services, the Azure AI Foundry Agent Service *does not* generate the response and return it to us right then and there - instead, it invokes the inference process (starting it), and then responds to let us know inference has now begun. The response we get, shown below, confirms that.

```
{
  "id": "run_khg7A3BJcuw58Igahi9Y3qL8",
  "object": "thread.run",
  "created_at": 1752591435,
  "assistant_id": "asst_SUiBkwY2MsknWcBfWo5tfEtc",
  "thread_id": "thread_L9HRkMrJcHy7ZOiR9I4J6ckY",
  "status": "queued",
  "started_at": null,
  "expires_at": 1752593235,
  "cancelled_at": null,
  "failed_at": null,
  "completed_at": null,
  "required_action": null,
  "last_error": null,
  "model": "20250715-gpt-4.1-mini",
  "instructions": "Talk like you are a cowboy.",
  "tools": [],
  "tool_resources": {},
  "metadata": {},
  "temperature": 1,
  "top_p": 1,
  "max_completion_tokens": null,
  "max_prompt_tokens": null,
  "truncation_strategy": {
    "type": "auto",
    "last_messages": null
  },
  "incomplete_details": null,
  "usage": null,
  "response_format": "auto",
  "tool_choice": "auto",
  "parallel_tool_calls": true
}
```

## Step 6: Check Run Status
With our thread now running (inference is now taking place), it will only be a matter of seconds before inference is complete (the AI's response is fully generated and available for retrieval).

Next, we can check the status of our thread to see what is happening right now: is the response still being generated or is it complete and ready for me to retrieve?

To check the status of our run, we can pass the run ID (`id` property in the previous response) into an endpoint like below:

```
GET https://testagents-resource.services.ai.azure.com/api/projects/TestAgents/threads/thread_L9HRkMrJcHy7ZOiR9I4J6ckY/runs/run_iYBOF6sioEhChPhnWtA4nuOJ?api-version=2025-05-01
Authorization: Bearer <<YOUR_ACCESS_TOKEN>>
```

As seen above, we are checking the status of run `run_iYBOF6sioEhChPhnWtA4nuOJ` on thread `thread_L9HRkMrJcHy7ZOiR9I4J6ckY` (that used agent `asst_SUiBkwY2MsknWcBfWo5tfEtc`, but that isn't noted here).

The API will return something like below:

```
{
  "id": "run_iYBOF6sioEhChPhnWtA4nuOJ",
  "object": "thread.run",
  "created_at": 1752587901,
  "assistant_id": "asst_SUiBkwY2MsknWcBfWo5tfEtc",
  "thread_id": "thread_L9HRkMrJcHy7ZOiR9I4J6ckY",
  "status": "completed",
  "started_at": 1752587902,
  "expires_at": null,
  "cancelled_at": null,
  "failed_at": null,
  "completed_at": 1752587903,
  "required_action": null,
  "last_error": null,
  "model": "20250715-gpt-4.1-mini",
  "instructions": "Talk like you are a cowboy.",
  "tools": [],
  "tool_resources": {},
  "metadata": {},
  "temperature": 1,
  "top_p": 1,
  "max_completion_tokens": null,
  "max_prompt_tokens": null,
  "truncation_strategy": {
    "type": "auto",
    "last_messages": null
  },
  "incomplete_details": null,
  "usage": {
    "prompt_tokens": 32,
    "completion_tokens": 94,
    "total_tokens": 126,
    "prompt_token_details": {
      "cached_tokens": 0
    }
  },
  "response_format": "auto",
  "tool_choice": "auto",
  "parallel_tool_calls": true
}
```

We can see in the response above that `status` is `completed`, meaning the run is complete - the AI's response is now fully generated and available for us to retrieve!

## Step 7: Retrieve Assistant Message
The final step in this process is retrieving the AI's message from the thread. To do this, we can query the `/messages` of the thread we have been working in, thread `thread_L9HRkMrJcHy7ZOiR9I4J6ckY`, like so:

```
GET https://testagents-resource.services.ai.azure.com/api/projects/TestAgents/threads/thread_L9HRkMrJcHy7ZOiR9I4J6ckY/messages?api-version=2025-05-01
Authorization: Bearer <<YOUR_ACCESS_TOKEN>>
```

This will return a full list of every message so far in this thread - both messages we have added (as "the user") and messages that assistant has generated and sent back to us.
```
{
  "object": "list",
  "data": [
    {
      "id": "msg_6FO8jotVRSLyPD6t4ehT4QGE",
      "object": "thread.message",
      "created_at": 1752587902,
      "assistant_id": "asst_SUiBkwY2MsknWcBfWo5tfEtc",
      "thread_id": "thread_L9HRkMrJcHy7ZOiR9I4J6ckY",
      "run_id": "run_iYBOF6sioEhChPhnWtA4nuOJ",
      "role": "assistant",
      "content": [
        {
          "type": "text",
          "text": {
            "value": "Well, partner, the sky's blue 'cause of a lil' thing called Rayleigh scatterin'. You see, sunlight's made up of all colors, but when it hits the Earth's atmosphere, the shorter blue wavelengths get scattered all around more than the other colors. That scattering spreads the blue light across the sky, makin' it look like a big ol' blue dome over our heads. Ain't nature somethin' mighty fine? Yeehaw!",
            "annotations": []
          }
        }
      ],
      "attachments": [],
      "metadata": {}
    },
    {
      "id": "msg_zE1gyAZnopYj2SQ5zn8vTyWr",
      "object": "thread.message",
      "created_at": 1752587787,
      "assistant_id": null,
      "thread_id": "thread_L9HRkMrJcHy7ZOiR9I4J6ckY",
      "run_id": null,
      "role": "user",
      "content": [
        {
          "type": "text",
          "text": {
            "value": "Why is the sky blue?",
            "annotations": []
          }
        }
      ],
      "attachments": [],
      "metadata": {}
    }
  ],
  "first_id": "msg_6FO8jotVRSLyPD6t4ehT4QGE",
  "last_id": "msg_zE1gyAZnopYj2SQ5zn8vTyWr",
  "has_more": false
}
```

As seen above, the underlying LLM, GPT-4.1-mini in this scenario, answered our question of *"Why is the sky blue?"* with *"Well, partner, the sky's blue 'cause of a lil' thing called Rayleigh scatterin'. You see, sunlight's made up of all colors, but when it hits the Earth's atmosphere, the shorter blue wavelengths get scattered all around more than the other colors. That scattering spreads the blue light across the sky, makin' it look like a big ol' blue dome over our heads. Ain't nature somethin' mighty fine? Yeehaw!"*.

Our agent correctly followed the instructions of "speak like a cowboy" we entered when making the agent.

**Note**: In the example above, the response from the LLM was *a message*. Developers that are familiar with LLMs will know this may not always be the case - sometimes an LLM will choose to invoke a tool; if a tool was invoked, you instead would see a different format provided!

## Continuing the Conversation
Continuing the conversation with our agent is very easy! Just simply go back to step 4 to add another message and proceed from there!

Again this is where any experience you may have with Azure OpenAI service or any other LLM service may differ slightly: instead of us providing the entire chat history to the agent each and every time we call, we **only have to provide the next message** again. The thread retains full chat history in the cloud, so the API is not "stateless" - it retains a history of all messages for us and those are still held and considered in the agent's generation of messages.