# Zero Trust Dataverse: Securing Dataverse API Integrations with Service Principals and Application Users

![banner](https://i.imgur.com/v6oF4gR.png)

In 2026, the stakes for data security have never been higher. As we navigate the age of Agentic AI and autonomous workflows, the data you store is no longer just a record: it is the "brain" fueling your company's intelligence. Protecting this mission-critical data is vital.

Microsoft Dataverse has emerged as the secure standard for businesses in this AI-driven era. For organizations looking to bridge the gap between innovation and safety, Dataverse provides a Zero Trust framework where every interaction is scrutinized and every identity is verified.

One of the most powerful ways to maintain this security posture is using Azure Service Principals paired with Dataverse Application Users. This combination allows your external services and AI agents to interface with your data using fine-grained permissions.

In this guide, I'll walk through how to set up this integration:

## Step 1: Create Service Principal (App Registration) in Azure
The foundation of a secure integration is a distinct identity. We start by visiting [the Azure Portal](https://portal.azure.com/#home) to create an App Registration. This creates a Service Principal, which is a dedicated identity for your application that exists independently of any human user. By using a Service Principal instead of a service account, you move away from vulnerable passwords and toward a model of explicitly defined permissions, ensuring your app has exactly the access it needs to your tenant and nothing more.

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

## Step 2: Register the Service Principal as an Application User in Power Platform
To actually interact with your data, Dataverse uses a specialized gateway called an Application User. This is a crucial security boundary: it allows you to map your Azure Service Principal to a specific environment and govern it using Dataverse Security Roles. By using an Application User instead of a standard licensed user, you ensure that this programmatic access is non-interactive, audit-ready, and adheres to the principle of least privilege within your organization's data.

Visit the [Power Platform Admin Center](https://admin.powerplatform.microsoft.com/), select your environment, go to "Settings" (at the top), and then select "**Application Users**" under "Users + permissions". This will open a list of application users registered within this particular Dataverse environment.

![app users setting](https://i.imgur.com/cI48Kjt.png)

At the top, select "+ New App User":

![new app user](https://i.imgur.com/TPGq5rq.png)

A fly-out pane will appear on the right. It will prompt you to select an "App". It is referring to the **Application User** (service principal) you set up in step one!

![add an app](https://i.imgur.com/zKN4BJJ.png)

Select **Add an app** which will populate with a list of app registrations in your tenant. Find the one you made in Step 1, then click "Add".

For "Business Unit", simply select whatever business unit this service principal should belong to (I used the default).

![select app](https://i.imgur.com/ftMMnQv.png)

Next, we will give this application user (service principal) **security roles**. Just like we can control the data privileges or a user (what they can/cannot do, can/cannot see, etc.), we can also control the privileges of a programmatic service principal via security roles!

To keep things simple, I gave my service principal the **System Administrator** role:

![ready to add](https://i.imgur.com/qFqJxrz.png)

After clicking "add" at the bottom, your application user is now ready!

![ready to go](https://i.imgur.com/YbYbWq9.png)

## Step 3: Get an Access Token
With our identity established in Azure and our permissions defined in Dataverse, we now will request and acquire an **access token**. Instead of sending your Client Secret with every data request (which would be a massive security risk) your application will make a single, secure POST request to Microsoft Entra ID.

Entra ID will verify your Service Principal's credentials and issue a short-lived Bearer Token. This token acts like a digital "key card". It is cryptographically signed, restricted to your specific Dataverse environment, and will automatically expire, drastically reducing the "blast radius" if the token were ever intercepted.

The following steps consists of API calls, which I will be showing in Bruno (similar to Postman).

We will make a `POST` request to a URL that will look like this:

```
https://login.microsoftonline.com/<TENANT_ID>/oauth2/v2.0/token
```

Plug in your **Directory (tenant) ID** that you collected in step! 

Then, we will provide various bits of information in the body, **Form URL Encoded**:
- Application (Client) ID *(from step 1!)*
- Client Secret value *(from step 1!)*
- A Scope (we are specifying *"I want to access this Dataverse environment please"*)
- A Grant Type (we are saying *"I'm asking for credential I can use in future requests please"*)

Your request will look something like this:

![get token](https://i.imgur.com/Db9Aj9v.png)

If all is successful, as you can see on the right, the Entra ID service will return `200 OK` and give us an `access_token` value. *That* is the value we want! That is permission (at least for a period of time... it expires!) to use the Foundry service! Take note of that `access_token` value (starts with `eyJ0eXAiOi...` in my example)!

## Step 4: Interface with the Dataverse API
With your short-lived access token in hand, you are now ready for the final step: use the Dataverse API! In the Zero Trust model, this is the "Continuous Verification" phase. Every single request you send from here on out will include that token in the header, allowing Dataverse to verify your identity and enforce your security roles in real-time.

Because Dataverse is built on the [OData v4.0 standard](https://www.odata.org/), interacting with it feels familiar to any modern developer, but with the added peace of mind that every `GET`, `POST`, and `PATCH` is being governed by the industry's most robust security engine.

In all subsequent API calls, provide the access token acquired from step #3 as a **bearer token** (header `Authorization: Bearer <token here>`):

![bearer](https://i.imgur.com/0rDcL2l.png)

Querying the Dataverse API is easy! Dataverse supports the [OData protocol](https://www.odata.org/), a standard for building and consuming REST APIs.

For example, to list all tables in the environment:

```
GET https://org7029e97c.crm.dynamics.com/api/data/v9.2/
Authorization: Bearer eyJ0eXAiOi...
```

Will return a list of all tables:

```
{
  "@odata.context": "https://org7029e97c.crm.dynamics.com/api/data/v9.2/$metadata",
  "value": [
    {
      "name": "aadusers",
      "kind": "EntitySet",
      "url": "aadusers"
    },
    {
      "name": "accounts",
      "kind": "EntitySet",
      "url": "accounts"
    },
    {
      "name": "aciviewmappers

...
```

Or to query all `contact` records:
```
GET https://org7029e97c.crm.dynamics.com/api/data/v9.2/contacts
Authorization: Bearer eyJ0eXAiOi...
```

For a full tutorial on performing all CRUD operations on Dataverse (Create, Read, Update, and Delete), see my earlier article on the Dataverse web API [here](https://timhanewich.medium.com/dataverse-web-api-101-97e4099ff10b)!

## Summary
By following these four steps, you have moved away from legacy, password-dependent integrations and into a Zero Trust architecture. Your application now has its own unique identity (the Service Principal), its own scoped permissions (the Application User), and uses short-lived, encrypted credentials (the Access Token).

Why this is the 2026 Gold Standard:
- **Identity-First Security**: You are treating your code as a first-class citizen with its own identity, making it easier to monitor and govern than shared service accounts.
- **Reduced Blast Radius**: By using Bearer Tokens that expire, you ensure that even a successful interception of a credential has a very limited "shelf life."
- **Complete Auditability**: Every row changed or deleted via the Web API is now explicitly stamped with the name of your Service Principal, providing a clear "paper trail" for compliance.

*Written by [Tim Hanewich](https://www.linkedin.com/in/timhanewich/), Sr. AI Business Solutions Engineer at Microsoft. Follow me [on GitHub](https://github.com/TimHanewich) for more of my content.*