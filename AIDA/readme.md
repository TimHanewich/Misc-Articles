# Writing my own CLI-based LLM Client: AIDA
Before I do nearly anything nowadays, I always check with my personal guide, *Copilot* or *ChatGPT*. These AI-driven assistants are invaluable in nearly every facet of life - tech assistance, financial advice, email drafting, and more.

However - I had a few problems. For one, to fire one up, I had to go to a special website in a web browser interface. Secondly, I found they were often not capable of doing *exactly* what I wanted, leaving me wishing for more customization control.

So, I developed **AIDA**, a command-line-interface (CLI) accessible Large Language Model (LLM) client. AIDA, short for **AI** **D**esktop **A**ssistant, is a lightweight CLI LLM client that allows me to interface with my own unique deployment of an LLM directly from my command line.

As an desktop-based AI client, I gave AIDA desktop-specific tools like the ability to open, read, and save files, navigate through folders on my computer, and reading webpages. Given my background and interest in finance, I also developed add-on "packages" that allow AIDA to surf billions of financial data points available by any publicly traded company, but I'll save that for another article.

In this article I will detail what AIDA is, how it works, and how you can use it too.

## What is AIDA?
At it's core, AIDA is a .NET 9.0 (C#) Console App - a simple command line utility that allows for you to interface with either a **local or remote** large language model connection. Being .NET-based, AIDA runs smoothly on both Windows and Linux - this was very important for me accessing a LLM in a Linux environment (oftentimes without a GUI!) was a necessity!

As mentioned above, AIDA allows for you to integrate with *your own* deployment of a remote or local model. As of the time of this writing, you can connect AIDA with any **Azure OpenAI** deployment or **Ollama** model you have; future plans include the addition of OpenAI and Azure Agent Service. For instance, after spinning up a deployment of `GPT-4.1` or `o4-mini` on Azure OpenAI service in Azure AI Foundry, you can give your deployment information (endpoint and API key) to AIDA and begin using it for inference!

AIDA, being a desktop-based assisant, also has the ability of reading files and navigating folders on your desktop. Unlike with web-based LLM clients where you have to *upload* files, AIDA is capable of reading any file on your computer - just give it the path to your file and it will use its `read_file` tool to read its content! Additionally, it can save content to your computer (a save for later note) as well. It can also open folders on your computer and thus navigate through your PC to complete a task, at your discretion of course.

Finally, AIDA also allows you to have clear visibility into your **token consumption**. Input and output tokens are the cost driver to any LLM use, so AIDA makes it easy to track how many tokens you have used up in any given session: just run the `tokens` command!

## Example: Adding Model Connection

## Example: Read file content
- Allows for entire context window to be used

## Example: Save file content

## Example: Navigate folder structure

## Example: Load/Save Chat

## How to install