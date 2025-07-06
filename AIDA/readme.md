# Writing my own CLI-based LLM Client: AIDA
Before I do nearly anything nowadays, I always check with my personal guide, *Copilot* or *ChatGPT*. These AI-driven assistants are invaluable in nearly every facet of life - tech assistance, financial advice, email drafting, and more.

However - I had a few problems. For one, to fire one up, I had to go to a special website in a web browser interface. Secondly, I found they were often not capable of doing *exactly* what I wanted, leaving me wishing for more customization control.

So, I developed **AIDA**, a command-line-interface (CLI) accessible Large Language Model (LLM) client. AIDA, short for **AI** **D**esktop **A**ssistant, is a lightweight CLI LLM client that allows me to interface with my own unique deployment of an LLM directly from my command line.

As an desktop-based AI client, I gave AIDA desktop-specific tools like the ability to open, read, and save files, navigate through folders on my computer, and reading webpages. Given my background and interest in finance, I also developed add-on "packages" that allow AIDA to surf billions of financial data points available by any publicly traded company, but I'll save that for another article.

In this article I will detail what AIDA is, how it works, and how you can use it too.

## What is AIDA?
At it's core, AIDA is a .NET 9.0 (C#) Console App - a simple command line utility that allows for you to interface with either a **local or remote** large language model connection. Being .NET-based, AIDA runs smoothly on both Windows and Linux - this was very important for me accessing a LLM in a Linux environment (oftentimes without a GUI!) was a necessity!

![AIDA screenshot of basic question answering i.e. why is the sky blue?]()

As mentioned above, AIDA allows for you to integrate with *your own* deployment of a remote or local model. As of the time of this writing, you can connect AIDA with any **Azure OpenAI** deployment or **Ollama** model you have; future plans include the addition of OpenAI and Azure Agent Service. For instance, after spinning up a deployment of `GPT-4.1` or `o4-mini` on Azure OpenAI service in Azure AI Foundry, you can give your deployment information (endpoint and API key) to AIDA and begin using it for inference!

AIDA, being a desktop-based assisant, also has the ability of reading files and navigating folders on your desktop. Unlike with web-based LLM clients where you have to *upload* files, AIDA is capable of reading any file on your computer - just give it the path to your file and it will use its `read_file` tool to read its content! Additionally, it can save content to your computer (a save for later note) as well. It can also open folders on your computer and thus navigate through your PC to complete a task, at your discretion of course.

Finally, AIDA also allows you to have clear visibility into your **token consumption**. Input and output tokens are the cost driver to any LLM use, so AIDA makes it easy to track how many tokens you have used up in any given session: just run the `tokens` command!

![AIDA screenshot of tokens]()

## Example: Adding Model Connection
AIDA gives you the ability to add, manage, and swap between multiple LLM reference connections, like Azure OpenAI or Ollama.

To add a connection reference, simply enter in the command `settings`, select "Add/Change/Delete a model connection", add your model connection, and save!

![AIDA screenshot of adding model connection]()

After adding your model connection, each time you select "Add/Change/Delete a model connection" in the settings page again, you will be presented with a list of model connections available. Select "Change Active Model Connection" to select a different LLM deployment to use in your subsequent conversation with the AI (all chat history will be ported over)

![AIDA screenshot showing all available model connections]()

## Example: Read file content
AIDA is primarily designed to be a lightweight desktop-based assistant. As such, it is capable of reading the contents of files (txt files, PDFs, and Word documents work right now - may add other file types in the future). 

In its base state, AIDA has several **tools** available to it that it can call to to accomplish the task that you have asked of it. One of those tools, `read_file`, allows AIDA to read the contents of a file on your PC - all it needs for you to do is provide it with the path to the file!

![AIDA screenshot asking it to summarize the contents of a file]()

While this is a rather simple trick (it is simply tool-calling and then a function I wrote to scrape the content and provide it to the LLM), this is a *big* advantage over the commercial LLM clients like Copilot, ChatGPT, Gemini, Claude, and others. While those clinets allow you to upload files, they usually do not provide *the entire* file's content to the LLM. Instead, in an effort to save on token consumption, they use a search layer *on top of* the file, specifically trimming content that a rudimentary search algorithm thought would be applicable to your query, and provide *that* to the model. While results are usually satisfactory, sometimes things can be missed... and that also makes prompts like "summarize this 30-page essay into 3 paragrahs for me" nearly impossible as the entirety of the 30-page essay won't truly be "seen" by the model!

## Example: Save file content
AIDA can also save files to your computer. Whenever you like, ask AIDA to save something to your computer and watch as it saves a `.txt` file to your local computer and populates it with content!

![AIDA screenshot of saving file content]()

![screenshot of that content]()

All files will be saved in the `Downloads` folder of your computer (this applies to both Windows and Linux)

## Example: Navigate folder structure
AIDA also has the ability to "open folders", meaning peer inside a folder and see its contents (a list of files and sub-folders in that folder). LLMs are so smart that they understand they can *chain this commnand together* to explore the entire folder structure of a PC, bouncing back and forth in your folders to accomplish whatever task you gave it!

For example, if you ask the LLM to summarize every reference of "Lake Dora" in your `./trips` folder, it knows that if it found a folder named `lake_trips` *within* the `./trips` folder, it can *also* peer inside the contents of that folder by re-running the `open_folder` tool, but this time opening the folder `./trips/lake_trips`.

This ability allows your LLM to explore multiple files and sub-folders in an effort to accomplish whatever task you gave it.

![AIDA screenshot of navigating folder]()


## Example: Save/Load Chat
Finally, AIDA also provides the ability to save chat history (a conversation) and then re-load it at a later time. Instead of doing something fancy like storing the history in the cloud, requiring you to sign in, scroll through a long list, etc., AIDA keeps things simple and puts the control into *your* hands.

Simply run the command `save` and AIDA will save a `.json` record of your chat history to your computer's local `Downloads` folder.

![screenshot of saving a chat history]()

Then, to load the chat history, run the `load` command and then provide AIDA with the path to that `.json` record of the chat history and the full context of the conversation will be loaded into memory - it will be like picking up where you left off!

## How to install
AIDA is open-source and available 100% free!

Visit [AIDA on GitHub](https://github.com/TimHanewich/AIDA) for a list of all releases of AIDA (both for Windows and Linux) and instructions on how to install!