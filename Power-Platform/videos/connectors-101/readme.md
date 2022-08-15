# Custom Connectors in 5 Minutes

### What is a connector?
In the Power Platform, Power Apps and Power Automate, the way that our apps and workflows can communicate externally with other services is through **Connectors**. You can think of a connector as a proxy, or a wrapper around another web API system. Before I dive deeper into that, let's take a step back and ask: What is an API?

### What is an API?
An API is an **Application Programming Interface**. Generally speaking, an API is a software intermediary that allows your program to interface with that service.  There are multiple types of API's, but the API's that we're talking about in the context of the Power Platform are web-based API's.

### API Request Structure
So how would you "talk" with a web API? A web API can be *"talked to"* using HTTP requests. This is similar to how you would send a request and expect a webpage response from Facebook when you go to Facebook.com in your web browser. HTTP, or Hypertext Transfer Protocol, is a globally-adopted protocol for data transfer over the web. Every HTTP request has 4 primary components:

- **Method** - this tells the API what you are trying to do. For example, if you're trying to request data, it would be GET. If you're trying to send data, it would be POST. There are many more, but this is enough to get an idea for now.
- **URL** - The website or web-service endpoint where the API "lives"
- **Headers** - Snippets of information that add context to your request - like who you are, what type of data you are sending, what type of data you are expecting to receive back, and more.
- **Body** - If you're **sending** data to the API, this is where you would include the content that you are sending - this can be binary content like a file, or data in a common format like JSON.

### Connectors: Consolidating the API request process
You can probably already see that "talking" with API's can at times be a cumbersome task. To connect with the Twitter API, for exampe, you'd have to know the specific format that the Twitter web API expects: what method to use, what URL to send the request to, any headers that need to be included, authorzation, how the body/content should be encoded, there is a lot to think about. This is a task probably best left to the professional developers. 

However, Microsoft still wants you, as a business developer, to have the tools at your disposal to use other services through API's in your apps and workflows. To allow for you to do this in an easy way, Microsoft abstracts the HTTP Request process away from you with *Connectors*. Connectors make communicatin with an API service easy; they handle all of the tedious things involved in constructing an HTTP request, and only present to you the things that you want to control.

Let's go back to the example of the Twitter API. Let's say I wanted to post a new Tweet from a Power App. I would use the "Post a Tweet" action from the out-of-the-box Twitter connector. This action would only ask me what I want to Tweet. The connector will take care of the rest of the HTTP request. Meaning that the method, URL, headers, structure, this will be taken care of for me by the connector. It would then send this request to Twitter on my behalf. By using the Twitter connector, I don't have to worry about the complicated process of constructing an HTTP request myself. Instead I focus on what is important to me and spend the time I've saved on app development.

### The Ecosystem
Connectors have been a very successful feature in Microsoft's low-code/no-code development platform, the Power Platform. The number of connectors available for use in the platform has grown from about 200 in 2019 to over 700+ at the time of this video.

The out-of-the-box connectors available in the Power Platform got there in one of two ways.
1) the first way is Microsoft themsleves made the connectors. This is most common with Microsoft's first party apps like Teams, Office, and various Azure services.
2) The other way is the 3rd party company that owns the service creates a connector themselves and certifies it for public use with Microsoft. For example, DocuSign wanted their API to be consumable through the Power Platform so they developed their own connector for handling communication with the DocuSign API. They, like all others, followed a rigurous certification process to have their connector made generally available as an out-of-the-box connector in the Power Platform. 

### Summary
So now I hope you see the purpose and benefit of using connectors in the Power Platform. There is a lot more to talk about when it comes to connectors, but I hope this helped you get started. 

Stay tuned for a future video I will make covering connector "Actions" via a webhook architecture and how you can design your own custom connector from within the Power Platform.