# Zero Trust Dataverse: Securing Web API Integrations with Service Principals and Application Users

[Some preamble about Dataverse's security posture and how it can be securely interfaced/integrated with using Azure Service Principals and the special 'Application User' feature within Dataverse, specifically designed for non-user based interaction]

## Step 1: Create Service Principal (App Registration) in Azure
The first step is to visit [the Azure Portal](https://portal.azure.com/#home) to create your *app registration* - this serves as a registration of the particular app/service you are building that your app/service will use to authenticate into your tenant. By having this app registration (service principal) as mechanism, we can ensure fine-grain control over exactly what the app can and cannot do in your tenant.

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
The Power Platform has a special mechanism for registering access for non-users (i.e. programmatic applications or services that wish to access Dataverse *not* on behalf of a user): [Application Users](https://learn.microsoft.com/en-us/power-platform/admin/manage-application-users)

Visit the [Power Platform Admin Center](), select your environment, go to "Settings" (at the top), and then select "**Application Users**" under "Users + permissions". This will open a list of application users registered within this particular Dataverse environment.

![app users setting](https://i.imgur.com/cI48Kjt.png)

At the top, select "+ New App User":

![new app user](https://i.imgur.com/TPGq5rq.png)

A fly-out pane will appear on the right. It will prompt you to seelct an "App". It is referring to the **Application User** (service principal) you set up in step one!

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
Now that we have created our service principal, registered as an **app registration** in Azure, and added our service principal as an authorized **application user** within our Power Platform environment (along with a security role), now we are ready to reach out to the Microsoft Entra ID service to request an **Access Token**.

We will make an HTTP call to the Entra ID authentication service to authenticate; upon authenticating, we will be provided with an Access Token that we can then use for all subsequent requests (i.e. reads, writes) to Dataverse for a period of time until it expires.


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
Now that we have acquired an access token, we can now go ahead and begin using the Dataverse API!

In all subsequent API calls, provide the access token acquired from step #3 as a **bearer token** (header like `Authorization: Bearer <token here>`):

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