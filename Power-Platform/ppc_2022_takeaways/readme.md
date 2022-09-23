# 8 Takeaways from the Inaugural Microsoft Power Platform Conference in Orlando (2022)

Last week I had the privlege of attending the very first Microsoft Power Platform Conference in Orlando Florida. This in-person, three day conference featured several keynotes from Microsoft leadership like Charles Lamana, Jeff Teper, and Scott Hanselman, 120 breakout sessions on all parts of the Power Platform, and even optional hands-on workshops to participate in. In addition to the fun I had, I've learned a lot and am documenting my **top eight** takeaways from the conference below.

## 1: Fusion development will become a major topic
I was surprised to see "pro-code" (the low-code community's term for traditional typed programming languages like C#, JavaScript, Python, etc.) have such a major role in a conference that is focused on the power of low-code/no-code development with the Power Platform. *Fusion development*, a growing topic, occurs when professional developers using pro-code languages unite with citizen developers using low-code frameworks to build solutions. The synergies of these *fusion teams* allow businesses to build better applications using both the technical expertise of the professional developer and the domain expertise of the citizen developer. 

Fusion development was a major theme at the conference this year. Many of the conference's 120 session focused on the synergies between Azure & Power Platform together and the benefits of fusion teams. For example:

- *Take advantage of Co-authoring via Git in Power Apps Canvas Apps*
- *Better together: Power Platform + Azure*
- *Power Platform for code-first developers & ISV's*
- *Using custom connectors with PVA*
- *Compliment Low-Code/No-Code with your Pro-Code Skills*
- *ALM for Citizen Devs and Code-First developers*

Given my background in traditional development, I was pleased to see this and chose sessions that generally covered this topic. Besides the sessions, even the keynotes (every conference attendee attended) were heavily focused on **both Azure and Power Platform**: the *It is a great time to be a cloud developer* keynote that Scott Hanselman and Julia Kasper delivered to close the conference covered everything from Azure API Management, .NET MAUI, Azure App Service, the latest C# innovations, Power Platform custom connectors, and of course, Power Apps.

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

## 4: Canvas & Model-Driven App Convergence

## 5: Developers, developers, developers!

## 6: Microsoft is committed to the Power Platform

## 7: Features & Functionality on my radar

## 8: It is a revolution