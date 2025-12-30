# How to use Microsoft Foundry via Entra ID Authentication (with Step-by-Step Screenshots)
As you already probably know from visiting this article, Microsoft Foundry (formerly known as *Azure AI Foundry* before November 2025) provides **two** ways to authenticate when using the service for various things, including LLM inference: **key-based**, and **Entra ID**.

## Entra ID Authentication: The Advantages
**Key-based** authentication is quick and simple... just grab the API key out of the Foundry portal, plug it in as a header under the `api-key` name, and you're up and running! However, **key-based** authentication, due to its simplicity, also has some limitations. One of the major limitations is **security**. While key-based authentication is simple, it is "all of nothing" access - anyone who has that key can perform any action the resource allows. 

With **Entra ID** authentication, you can use Microsoft Azure's **Role-Based Access Control (RBAC)**; this means you can assign specific roles to users or authorized applications within your tenant, following the principles of **least privileges**. You can also use **Conditional Access** to enforce policies that require Multi-Factor Authentication, a compliant device, or even a specific IP ranges before an AI model can be accessed.

## How to use Entra ID Authentication with Foundry
So how do we actually begin using Microsoft Foundry services via Entra ID Authentication? As mentioned above, it isn't quite as simple as key-based, but I will do my best to describe below, with screenshots! *Note: while I'll be showing you how to accomplish this with the "raw" HTTP-based API calls, you can likely find some SDK libraries out there that will facilitate some of this process for you in the programming language of your choice*.

### Step 1: Register Your Application (Service Principal)
All requests we will make to the Foundry service will be through a **service principal** - an application registered within our tenant.

Visit the Azure portal at [portal.azure.com](https://portal.azure.com) and navigate to the Microsoft Entra ID service.

![portal](https://i.imgur.com/EYXZ8aR.png)

Next, navigate to "App Registrations". This will be where we declare a new app registration (service principal).

![app reg](https://i.imgur.com/ARpDp4D.png)

Click on "+ New Registration" which will take you through some questions about your app registration. Give it a unique name and select "Accounts in this organizational directory only" (the first option) for supported account types. Leave the "Redirect URI" section blank.

![fill out app reg details](https://i.imgur.com/RbcmZSD.png)

Once you have created your new app registration, there are **three details** we will want to take note of: the **application (client) ID**, the **Directory (tenant) ID**, and a **client secret**. I blacked mine out for privacy, but take note of the application ID and directory ID on the main page!

![ids](https://i.imgur.com/7czDBeB.png)

Next, we will have to make a client secret for this app registration. On the left navigation, click on "Certificates & Secrets", then "Client Secrets", and then click "New Client Secret". 

![client secret](https://i.imgur.com/5C9AIbJ.png)

A flyout pane will appear on the right. Give your secret a description and then set an expiration date. Note: you will have to update the secret for your app registration and subsequent service when it expires.

![secret info](https://i.imgur.com/K5jfBS0.png)

After you create your secret, it will list it in the secrets list below. The **Value** is what we want! Hit the copy button next to your secret's value to copy it!

![secret value](https://i.imgur.com/fvQbZd2.png)

Fantastic! Now we have *three values* we will need from our app registration to move onto the next step! Those are...
- Your **Directory (tenant) ID**
- Your app registration's **Application (Client ID)**
- Your app registration's **Client Secret (value)**

We are now ready to move on to step 2!

## Step 2: Grant the Service Principal Access to Foundry
As mentioned earlier, Entra ID authentication uses **Role-Based Access Control** (RBAC) with **least-privilege principles**. Naturally, that app registration (freshly made) does not just get permissions to use *anything* across our Azure tenant! Instead, we as an admin must *grant* specific privileges to that app registration. And in this scenario, the thing we most want to grant it the privilege to use is Foundry!

To grant the app registration Foundry privileges, navigate to your Foundry deployments.

![foundry search](https://i.imgur.com/7C8zlOB.png)

You will see a list of your Foundry instances. Click on the one (you may only have one) that you wish to grant the app registration access to.

![click on first](https://i.imgur.com/YpOggKR.png)

Then, select "Access control (IAM)" on the left navigation pane. Click on "+ Add" and then click on "Add role assignment".

![IAM](https://i.imgur.com/Fzg9eTv.png)

This will open up a page with a long list of various roles (privileges) we can grant to various users or apps within our tenant. The privilege we are looking for today, the one that grants the ability to *use* models in this Foundry instance (called "inference"), is called **Cognitive Services User**!

Perform a search for "Cognitive Services" and you should see it appear in the list. Select it, then press "Next".

![search for privilege](https://i.imgur.com/YdKl6Jv.png)

The next page will ask you who (or what) you want to grant this privilege to. We want to grant it to a "User, group, or service principal", so select that option under the "Assign access to" toggle. Then click "+ Select Members" to open a flyout pane on the right, then search for your app registration you made in step #1! Select it, then press "Select" at the bottom.

![find AR](https://i.imgur.com/dPbye5P.png)

With your app registration added, click "Review + assign" at the bottom.

![review and assign](https://i.imgur.com/0ySC7BE.png)

Then click it again to assign it to your app registration. Congrats! That app registration now has permissions to use that Foundry resource. We are now prepared to move onto the next step.

## Step 3: Obtain an Entra ID Access Token
When using Entra ID, we first make a call to the Entra ID service *requesting* access to a resource in the Azure tenant (in this case, Foundry). So that is our next goal - call to Entra ID and say *"Hi, it is me, your app registration... may I use Foundry right now?"*. The formal way of doing this is requesting an **Access Token** from Entra ID. And this access token will be used in all future calls to Foundry as "proof" that we have access!

The following steps consists of API calls, which I will be showing in Bruno (similar to Postman).

We will make a `POST` request to a URL that will look like this:

```
https://login.microsoftonline.com/<TENANT_ID>/oauth2/v2.0/token
```

Plug in your **Directory (tenant) ID** that you collected in step! 

Then, we will provide various bits of information in the body, **Form URL Encoded**:
- Application (Client) ID *(from step 1!)*
- Client Secret value *(from step 1!)*
- A Scope (we are specifying *"I only want to use Foundry right now please"*)
- A Grant Type (we are saying *"I'm asking for credential I can use in future requests please"*)

Your request will look something like this:

![token request](https://i.imgur.com/Ve4n3uf.png)

If all is successful, as you can see on the right, the Entra ID service will return `200 OK` and give us an `access_token` value. *That* is the value we want! That is permission (at least for a period of time... it expires!) to use the Foundry service! Take note of that `access_token` value (starts with `eyJ0eXAi...` in my example)!

## Step 4: Use Foundry Services
With that `access_token`, we are now ready to finally begin consuming the Foundry services! Before we dive in, note that the `access_token` we were granted *does* expire. It generally lasts somewhere around an hour before you'll have to repeat step #3 again to request a new one.

In the example below, assume I have a `gpt-5-mini` deployed within my Foundry instance. If you don't know how to do this, see [here](https://learn.microsoft.com/en-us/azure/ai-foundry/foundry-models/how-to/deploy-foundry-models?view=foundry-classic) to first deploy a model that you can then use in the subsequent steps.

All you have to do now is provide that `access_token` collected in step 3 as a **Bearer Token** in all future API calls to the Foundry service! If you are here, you probably already know that those API calls look like (and probably how to provide it as a Bearer Token), but providing examples below as well for the sake of being clear. 😄

In the below example, I am calling to the Foundry service using OpenAI's *responses* API:

```
POST https://ai-testaistudio030597089470.openai.azure.com/openai/v1/responses
Content-Type: application/json
Authorization: Bearer eyJ0eXAi...

{
    "model": "gpt-5-mini",
    "input":
    [
        {
            "role": "user",
            "content": "Hello! Why is the sky blue?"
        }
    ]
}
```

In the example above, ensure you are providing *the entire* access token as the bearer token (I truncated mine for brevity).

And looking at this in Bruno (similar to Postman), I provided the `access_token` as a Bearer Token under the "Auth" tab (it knows to include it as a header in the bearer token format):

![bruno auth](https://i.imgur.com/FWlHPNA.png)

And then filled out the rest of the POST request accordingly:

![success](https://i.imgur.com/cFumc8S.png)

Congratulations! You are now consuming Foundry services using **Entra ID Authentication**!

## Summary
This article provides a technical walkthrough for transitioning from key-based to Entra ID authentication within Microsoft Foundry (formerly Azure AI Foundry). It highlights the shift toward more secure, enterprise-grade identity management for AI services.

The workflow, at a glance:
1. Register a service principal (app)
2. Assign Permissions to the service principal
3. Request & Obtain an access token
4. Consume the Foundry service using that token!

Now you are prepared to use Foundry by authenticating via Entra ID, a significantly more secure method than using static API keys. While a key can be easily leaked, hardcoded, or shared, Entra ID relies on short-lived access tokens that expire automatically, drastically reducing the window of opportunity for an attacker if a token is intercepted.

Written by [Tim Hanewich](https://www.linkedin.com/in/timhanewich/), Sr. AI Business Solutions Engineer at Microsoft. Follow me [on Github](https://github.com/TimHanewich) for more of my content.