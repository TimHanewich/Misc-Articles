# 8 Takeaways from the Inaugural Microsoft Power Platform Conference in Orlando (2022)

Last week I had the privlege of attending the very first Microsoft Power Platform Conference in Orlando Florida. This in-person, three day conference featured several keynotes from Microsoft leadership like Charles Lamana, Jeff Teper, and Scott Hanselman, 120 breakout sessions on all parts of the Power Platform, and even optional hands-on workshops to participate in. In addition to the fun I had, I've learned a lot and am documenting my **top eight** takeaways from the conference below.

## 1: Fusion development will become a major topic
I was surprised to see "pro-code" (the low-code community's term for traditional typed programming languages like C#, JavaScript, Python, etc.) have such a major role in a conference that is focused on the power of low-code/no-code development with the Power Platform. *Fusion development*, a growing topic, occurs when professional developers using pro-code languages unite with citizen developers using low-code frameworks to build solutions. The synergies of these *fusion teams* allow businesses to build better applications using both the technical expertise of the professional developer and the domain expertise of the citizen developer. 

Fusion development was a major theme at the conference this year. Many of the conference's 120 session focused on the synergies between Azure & Power Platform together and the benefits of fusion teams. For example, some of the session titles:

- *Take advantage of Co-authoring via Git in Power Apps Canvas Apps*
- *Better together: Power Platform + Azure*
- *Power Platform for code-first developers & ISV's*
- *Using custom connectors with PVA*
- *Compliment Low-Code/No-Code with your Pro-Code Skills*
- *ALM for Citizen Devs and Code-First developers*

Given my background in traditional development, I was excited to see this and chose sessions that generally covered this topic. Besides the sessions, even the keynotes (every conference attendee attended) were heavily focused on **both Azure and Power Platform**: the *It is a great time to be a cloud developer* keynote that Scott Hanselman and Julia Kasper delivered to close the conference covered everything from Azure API Management, .NET MAUI, Azure App Service, the latest C# innovations, Power Platform custom connectors, and of course, Power Apps.

It was very clear to me that, while we work with the Power Platform, a low-code/no-code development framework, collaboration with pro-dev counterparts will allow us to achieve even more. "Fusion teams" will be commonplace in the years to come.

## 2: The futuere of Power Fx is bright
The most impressive session I attended was *Best Practices for Using Power Fx in Power Apps and the Power Platform* by Greg Lindhorst. Greg, a Principal PM Architect and one of the "Founding Fathers" of Power Fx at Microsoft, covered some best practices for using Power Fx, and detailed some of the exciting up and coming features of this growing "language".

Some incredible new features that Greg and his team described and will be coming soon:
- **Natural Language Support** - instead of writing a formula, describe what you want. Greg gave the example of typing in "does customer start with bill?" to the formula bar. Power Fx used natural language processing to understand what you are describing, and suggested the formula **StartsWith(Customer, "bill")**. This is only a simple example, but it shows the direction in which Power Fx is going - even *more accessible* to citizen developers.
- **Smart Repair** - ever get an error message but have no idea what you did wrong or how to fix it? Error messages will now learn what you are trying to accomplish and suggest a syntactically correct formula to use instead of the incorrect one you have.
- **Named formulas** - like Microsoft Excel, Power Fx will now support *named formulas*. A "Formulas" property will now be available at the top-level of your app and will allow you to define formula variables that can later be used in the application.
- **Prefetching** - Power Fx will now intelligently decipher a series of functions and will run calls *concurrently*, not one after another. For the developer and user, this means less loading time.
- **ParseJSON** - Parsing JSON has never been possible in Power Apps... you would have to delegate the parsing to Power Automate and return the intended values to Power Apps. Now, Power Apps has it's own function, *ParseJSON*, for parsing a JSON document. The bottleneck that the Power Fx team had to address before they could address this was how one could handle untyped objects, or objects that did not have a definitive structure defined by a schema (like JSON). So, the team also added...
- **Untyped Objects** - You have never been able to handle an unstructured object without a schema in Power Apps (i.e. a JSON document, YAML file, etc). Now the tools to do that exist, and they are called *untyped objects*. Using untyped objects, you can now reference properties of an object that have not been declared by a schema; in other words, you can reference properties that Power Apps isn't sure even exist! This is key for allowing Power Apps to parse JSON using the aforementioned *ParseJSON* function.

Also, **Power Fx is now OPEN SOURCE**! All of the source code that drives Power Fx is now [available on GitHub](https://github.com/microsoft/Power-Fx). What is even cooler is the Power Fx team also released the Power Fx interpreter as a [nuget package](https://www.nuget.org/packages/Microsoft.PowerFx.Interpreter/) that you can download and consume in your own .NET projects. This means that you will have the ability to use the Power Fx interpreter and evaluation engine in your applications. Greg's team gave a short demo of using the interpreter in a C# console app:

(INCLUDE PICTURE OF POWER FX INTERPRETER IN C#)

Using Power Fx in a REPL (read-eval-print-loop) environment was also demonstrated via a console interface. I believe this was a console app that the Power Fx team set up that leverages the Power Fx library, but it also may have been a separate Power Fx app/CLI that they are working on... but more likely the former.

Finally, the team hinted that, despite *ParseJSON* being announced only recently, you would have been able to find unit tests of the function in the [open-source repository on GitHub](https://github.com/microsoft/Power-Fx) over 6 months ago. They hinted that you may find other future features if you watch the commit timeline closely...

## 3: Dataverse is evolving
Another of my favorite sessions was the *Every System Connected* session by Nirav Shah, the VP of Dataverse at Microsoft. Nirav spoke about the importance of data in today's organizations, highlighted a customer story, and the vision for the future of Dataverse.

The Dataverse team is investing heavily into improving the experience of using virtual tables. Virtual tables is not a new concept of Dataverse and [has been around since the early days of Dynamics 365](https://learn.microsoft.com/en-us/power-apps/maker/data-platform/create-edit-virtual-entities). This feature allows you to *substitute* another datasource for Dataverse while still using the same functionality you would benefit from in Dataverse: using it in Model Driven apps, forms, views, etc. This feature has often been overlooked and in the background as it has been seen only as a secondary functionality to Dataverse itself, the star of the show. However, the Dataverse team is changing this notion and investing into creating a new unified UI for establishing connection to other tabular data sources as part of the virtual table setup process.

With this new UI, you will be able to point Dataverse at your SharePoint list or SQL table, have it *appear* as a Dataverse table, and build an entire model driven app around it, as if it were an *actual* Dataverse table. This is nothing new, but Nirav went on to explain that their goal is the "virtualization of data", and for there to be a **consistent API interface for everything**, and Dataverse *is* that consisten API interface. In otherwords, Dataverse could servie as the enterprise's "front gate" for all data exchanges. Even if data ultimately lies in SQL or SharePoint, you can use the Dataverse API for all your CRUD operations.

In addition to the virtual tables investment, the team debuted their new *Formula* column type. What was previously a **calculated** or **rollup** field, will now be a **Formula Column**. Using formula columns, you will be able to describe any unique calculation that Dataverse should perform on data in the row(s) and save to this column, the formula column. Just like Excel, you can use any operator (addition, subtraction, multiplication, division) and leverage some simple functions or constants.

(INCLUDE PICTURE OF FORMULA COLUMN)


## 4: Canvas & Model-Driven App Convergence: Custom Pages
At the *Building Modern Power Apps* session by Adrian Orth, Leon Welicki, and Ramiro Melgoza, the vision for and a demonstration of the convergence of Canvas and Model-Driven apps was shown. This is a topic that has been long discussed - what app should you build? Weighing the various benefits and downsides of each, and choosing one for a particular function while having the other type for another. In the future, there will be only one single app type. Within that app, you will be able to create "custom pages", a reusable page that can be used across a model-driven app. These custom pages are designed in a familiar setting, using the same canvas maker portal you have already been using to make canvas apps. 

(INCLUDE PICTURE OF CUSTOM PAGE INTRO SLIDE)

The team sees the following common scenarios, but we are free to dream big here!
- Landing page
- Key data summaries/dashboards
- Quick access to common tasks

Furthermore, the custom page can be placed anywhere within the app. A pop-up dialog can be a canvas page; or, you can "anchor" a canvas page permanently on the side of the application so it is ever there, ever present.

(INCLUDE PICTURE OF CENTER DIALOG CUSTOM PAGE)

(INCLUDE PICTURE OF SIDE EMAIL PAGE)

## 5: Developers, developers, developers!
Whether you are a citizen developer of professional developer, it is clear that Microsoft's mission is to provide you with the *absolute best tools in the market* to develop your vision. I saw this at the conference along the entire development spectrum.

Microsoft is investing heavily into their business applications portfolio, and that is evident particularly in the Power Platform. In only a short 6-7 years, Power Apps (and the Power Platform) has evolved tremendously. The resources that have been allocated to this group has certaintly risen since then and we will see their efforts materialize. The next 6-7 years could be even more game-changing than the last.

On the *professional* developer side, investments are being made, but this is nothing new for Microsoft. The quip that caught my attention the most is hearing the following repeated several times: *We want .NET to be the best language for API's*. I heard this several times and saw the new technology that is backing up this claim:

**C# minimal API** - Showed off in several sessions, including the Scott Hanselman keynote. This is the latest upgrade to ASP.NET that allows you to create an API endpoint with only a few lines of code. For example:
```
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello World!");

app.Run();
```
You can read more about the **C# minimal API** [here](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis?view=aspnetcore-6.0).

**Port Tunneling** - When testing an API that you are developing in Visual Studio or VS Code, the endpoint you will call will always start with something like this: `http://localhost:7071`. That is because your API is running *locally*. When it is running locally, this means you will be able to call it yourself on your computer, but another computer from a different network would not be able to call it because it isn't *exposed* publicly. After deploying to a service like **Azure App Service** or **Azure Functions**, you would be given a publicly-accessible URL which serves as your API endpoint. 

This always made it difficult for testing with cloud services like the Power Platform. You were unable to run an entire end-to-end test, from the Power App/Automate "front end" consuming the API, to the actual code that is running the API. **Port Tunneling** is a new feature which, instead of serving on `localhost`, gives you a publicly-accessible endpoint with the code running in *real-time* on *your computer*! You can give the URL to a friend to try the service, insert a breakpoint in your code and see it being hit after being called (your friend would be awaiting a response in the meantime), or as shown at the conference, provide it to a cloud SaaS app/workflow to be tested on.

In the fusion team setting, this allows a citizen developer to collaborate in synchrony with a professional developer as that pro-dev developers an API for the citizen developer and tests it in their *actual* application in real-time. This is big!

## 6: A case study for Power Platform adoption at scale
Andrew Gaskins, the Global Power Platform Lead at Lumen Technologies, a global telecommunications company, was featured at the *Every System Connected* session by Nirav Shah, VP of Dataverse. Andrew told the story about his company's adoption of the Power Platform. Lumen's story provides us with a case study for how a large organization can use low-code/no-code to drive innovation at every corner of their business:

- Lumen's journey with the platform began in 2017 when their very first Power App was made.
- In 2020, interest in "Premium" connectors was shown from members of their business community.
- In late 2020, Dataverse for Teams began to gain traction
- In early 2021, Lumen and Microsoft team up and **Lumen Makers**, a community of Power Platform makers at Lumen that meets monthly, is formed.
- In June 2021, Lumen secures enterprise-wide Power Platform licenses (every employee)
- 2022 - Dataverse takes hold at Lumen

Andrew spoke about the importance of their *Lumen Makers* community and how collaboration is facilitated: A monthly in-person meetup that is tailored for all experience levels, from makers that are brand new and curious, to the professional development veterans. A Microsoft Teams team was created for the community and a channel was created as a forum for each part of the platform - Power Apps, Automate, PVA, Dataverse, etc.

Andrew detailed the benefits and value that has been created for Lumen as they've adopted the platform:
- Most value was realized after rolling out licenses **enterprise wide**. No longer did they have to worry about who is licensed and managing licensing when deploying an app. Despite the obvious cost of licensing the full organizaiton, the most *value* was realized when everyone was licensed. This allowed them to easily roll out enterprise-wide apps like a COVID attestion app, without worrying about licensing. 
- What won them over was the "pro-dev" features of the platform that would allow their traditional developers to leverage the platform for traditional workloads as well. For example, using ALM practices with Dataverse. The major features that Lumen's traditional developers embraced was the ability to use the **Dataverse Web API** to read and write data to Dataverse from their own applications written in "pro-code". Citizen makers could easily write to Dataverse while professional developers could consume that data (and data schema) in their own .NET applications.
- Lumen ❤️'s Dataverse for it's: Performance, Security, and Governance

Andrew's word of advice for any organization embracing the Power Platform is to have a plan in place for two things: internal upskilling and environment strategy (governance). 

## 7: Features & Functionality on my radar
There are several new-ish features that were mentioned, discussed, or demonstrated at the conference that I am excited to research further and learn more about:

- [Managed environments](https://learn.microsoft.com/en-us/power-platform/admin/managed-environment-overview) - Additional governance control over environments. New features include [weekly digests](https://learn.microsoft.com/en-us/power-platform/admin/managed-environment-weekly-digests), [sharing limits](https://learn.microsoft.com/en-us/power-platform/admin/managed-environment-sharing-limits), and [data policies](https://learn.microsoft.com/en-us/power-platform/admin/managed-environment-data-policies).
- The [Automation Kit](https://learn.microsoft.com/en-us/power-automate/guidance/automation-kit/overview/introduction) for Power Automate Desktop, used in CoE scenarios.
- **Power Pages** has a feature to open a site in VS Code, to allow for a pro-dev to edit the HTML/CSS/JS behind the site. 
- [Querying Dataverse using T-SQL with SSMS](https://learn.microsoft.com/en-us/power-apps/developer/data-platform/dataverse-sql-query)
- [Power Platform Tools Extension for VS Code](https://powerapps.microsoft.com/en-us/blog/power-platform-extension-for-vs-code/)
- [Power Platform Command Line Interface (CLI)](https://learn.microsoft.com/en-us/power-platform/developer/cli/introduction) for various administrative tasks in the Power Platform.
- [Portals (Power Pages) web API](https://learn.microsoft.com/en-us/power-apps/maker/portals/web-api-overview), an API that can be used within a Power Pages site for doing things like CRUD operations on Dataverse, creating an account, update a contact, or changing table permissions
- [Power Fx Named Formulas](https://powerapps.microsoft.com/en-us/blog/power-fx-introducing-named-formulas/) - This was covered in depth at a session I described above
- Power Up - Microsoft is investing into the upskilling and training of new makers with the Power Platform. This new program called "Power Up" is the place for anyone to start! Check it out: [aka.ms/powerup](https://aka.ms/powerup).

## 8: It is a revolution
The sessions and speakers were fantastic, but I was even more impressed with the folks in attendance. It is clear that this is a platform that is very near to the hearts of many of us, but especially those that are brand new to development entirely. Many had come from different industries and different careers entirely. People from all stages of career, from all industries, are making the switch to becoming a full-time Power Platform developer.

During one of the keynote sessions with engineering leadership, one of the attendees approached the microphone and, emotionally, thanked the team for creating the platform which she is now basing her career on. She said something along the lines of *"I'm 50 years old, have switched careers, and have FINALLY found my passion*". Many shared similar stories.

You see here how the Power Platform embodies Microsoft's mission statement, *To empower every person and every organization on the planet to achieve more*. The future is bright with no-code/low-code, and Microsoft's Power Platform is leading the way!