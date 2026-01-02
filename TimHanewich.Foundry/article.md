# How to Call Microsoft Foundry Services in C#
Building applications on top of Large Language Models (LLMs) has become a standard requirement for all modern developers, and that includes .NET developers. However, if you are working within the Microsoft ecosystem - specifically using Microsoft Foundry (formerly Azure AI Foundry) - the integration experience can sometimes feel heavier than it needs to be.

Standard SDKs are often designed to be "everything for everyone," resulting in complex boilerplate and deeply nested configurations. That's why I built **TimHanewich.Foundry**, a lightweight, purpose-built .NET library designed to make interfacing with Foundry deployments as clean and intuitive as possible.

In this article, I'll show you how to get up and running with Microsoft Foundry in C# using this library.

## Why TimHanewich.Foundry?
The biggest hurdle with official enterprise SDKs is the "cognitive overhead": the sheer amount of boilerplate code, deeply nested namespaces, and configuration objects you have to navigate just to send a simple prompt.

**TimHanewich.Foundry** was built for *simplicity*. While the official OpenAI or Azure SDKs are powerful, they are often designed for every possible edge case, which makes the learning curve steep. This library targets the specific nuances of the Microsoft Foundry service with a flat, intuitive architecture. It offers:
- **Native Entra ID Support**: Simplifies Service Principal authentication.
- **Smart Conversation Tracking**: Uses the `previous_response_id` natively to maintain context without manually managing arrays of message history.
- **Strongly-Typed Tooling**: Simple classes for function calling and JSON mode.
- **Zero Bloat**: Built for .NET 10+, it stays out of your way and lets you focus on the logic

## Getting Started
First, add the package to your project:
```bash
dotnet add package TimHanewich.Foundry
```

### 1. Basic Prompting
To call a model, you simply define your Deployment (endpoint and key) and a ResponseRequest.

```C#
using TimHanewich.Foundry.OpenAI.Responses;

// Define the deployment
Deployment d = new Deployment();
d.Endpoint = "https://your-resource.openai.azure.com/openai/responses?api-version=2025-04-01-preview";
d.ApiKey = "YOUR_API_KEY";

// Create a request
ResponseRequest rr = new ResponseRequest();
rr.Model = "gpt-5-mini"; 
rr.Inputs.Add(new Message(Role.developer, "Talk like a cowboy."));
rr.Inputs.Add(new Message(Role.user, "Hi! Why is the sky blue?"));

// Call the service
Response r = await d.CreateResponseAsync(rr);

foreach (Exchange exchange in r.Outputs)
{
    if (exchange is Message msg)
    {
        Console.WriteLine("Response: " + msg.Text);
    }
}
```

### 2. Handling Conversations (The Easy Way)
Maintaining state is usually the hardest part of LLM development. This library leverages the responses API's `previous_response_id` property to link messages together within a conversation context automatically. You don't need to pass the entire history back and forth; just pass the ID of the last response.

```C#
ResponseRequest rr = new ResponseRequest();
rr.Model = "gpt-5-mini";
rr.PreviousResponseID = "resp_05c65468f65bdb3c006950294d66948196ac0afea12bfba22d";  // Link to the previous exchange
rr.Inputs.Add(new Message(Role.user, "Can you explain that again, but for a 5-year-old?"));

Response r = await d.CreateResponseAsync(rr);
```

### 3. Function Calling (Tools)
The library provides a structured way to define tools. When the model needs to "call" a function, you receive a ToolCall object which you can then resolve and pass back.

```C#
// Define a tool
Tool CheckWeather = new Tool();
CheckWeather.Name = "CheckWeather";
CheckWeather.Description = "Check the weather for any zip code.";
CheckWeather.Parameters.Add(new ToolInputParameter("zip_code", "Zip code to check"));

//Add the tool
rr.Tools.Add(CheckWeather);

// Call the service
Response r = await d.CreateResponseAsync(rr);

// Check if the model wants to use the tool
foreach (Exchange exchange in r.Outputs)
{
    if (exchange is ToolCall tc)
    {
        Console.WriteLine($"Model requested tool: {tc.ToolName} with args: {tc.Arguments}");
    }
}
```

### 4. Enterprise Security with Entra ID
For production environments, API keys are often discouraged in favor of Managed Identities or Service Principals. The library includes a built-in `EntraAuthenticationHandler` to manage token acquisition and expiration.

```C#
EntraAuthenticationHandler eah = new EntraAuthenticationHandler();
eah.TenantID = "your-tenant-id";
eah.ClientID = "your-client-id";
eah.ClientSecret = "your-client-secret";

TokenCredential credential = await eah.AuthenticateAsync();

Deployment d = new Deployment();
d.Endpoint = "https://your-resource.openai.azure.com/...";
d.AccessToken = credential.AccessToken; // Use the token instead of a key!
```

## Conclusion
Interfacing with high-performance models in Microsoft Foundry shouldn't require hundreds of lines of configuration. With **TimHanewich.Foundry**, you get a streamlined, .NET-first experience that handles the heavy lifting of authentication and conversation state for you.

You can find the full source code and more examples [on GitHub](https://github.com/TimHanewich/TimHanewich.Foundry) or download the package [on NuGet](https://www.nuget.org/packages/TimHanewich.Foundry).

Happy coding!