# Writing My Own CLI-Based LLM Client: AIDA
![AIDA banner]()  
Before I do nearly anything nowadays, I always check with my personal guide - usually either *Copilot* or *ChatGPT*. These AI-driven assistants are invaluable in nearly every facet of life: tech assistance, financial advice, email drafting, and more.

However, I ran into a few issues. For one, firing one up meant heading to a special website in a browser. Second, they never quite did *exactly* what I wanted. I wanted more control and customization.

So, I developed **AIDA**: a command-line-interface (CLI) accessible Large Language Model (LLM) client. AIDA stands for **AI** **D**esktop **A**ssistant, and it’s a lightweight CLI LLM client that lets me interface with my own unique deployment of an LLM, right from the command line.

As a desktop-based AI client, I gave AIDA tools that make it truly feel at home: it can open, read, and save files, navigate through folders, and even read web pages. And, given my interest in finance, I started developing add-on “packages” so AIDA can surf billions of financial data points from publicly traded companies - but I’ll save that fun for another article.

In this article, I’ll detail what AIDA is, how it works, and how you can use it too.

## What is AIDA?
At its core, AIDA is a .NET 9.0 (C#) Console App - a simple command line utility that lets you interface with either a **local or remote** large language model connection. Because it’s .NET-based, it runs smoothly on both Windows and Linux, which was a must for me (since I often need LLM access in a Linux environment - sometimes without a GUI!).

![AIDA screenshot of basic question answering i.e. why is the sky blue?]()

What makes AIDA different is that you can connect it with *your own* deployment of a remote or local model. As of writing, you can hook AIDA up to any **Azure OpenAI** deployment or local **Ollama** model you have. (Future plans: add OpenAI and Azure Agent Service support!) For example, if you’ve got a deployment of `GPT-4.1` or `o4-mini` running on Azure OpenAI, just give AIDA the endpoint and API key, and you’re off to the races.

AIDA is designed to play nicely with files and folders on your desktop. No need to upload anything - just provide the path to a file, and AIDA’s `read_file` tool takes care of the rest. It can also save content to your computer, acting as your digital note-taker. Plus, AIDA can open folders and help navigate your file system whenever you want.

And, because tokens are the cost driver for LLMs, you get a handy `tokens` command that shows exactly how many tokens you’ve used in any session. No surprises!

![AIDA screenshot of tokens]()

## Example: Adding Model Connection
AIDA gives you full control to add, manage, and swap between multiple LLM connections, like Azure OpenAI or Ollama.

To add a connection reference, just type `settings`, select “Add/Change/Delete a model connection,” add your connection, and save!

![AIDA screenshot of adding model connection]()

Once you’ve added connections, you’ll see a list of available models each time you visit “Add/Change/Delete a model connection.” Select “Change Active Model Connection” to instantly switch the AI model you’re chatting with (and yes - all your chat history comes along for the ride).

![AIDA screenshot showing all available model connections]()

## Example: Read File Content
AIDA is built as a lightweight desktop assistant, so it can read file contents out-of-the-box - TXT, PDFs, Word docs (and maybe more file types down the road!).

AIDA uses tools to complete tasks. One, `read_file`, lets it read any file - just tell it the file path!

![AIDA screenshot asking it to summarize the contents of a file]()

This sounds simple (it’s just tool-calling under the hood), but it’s a big advantage over commercial LLM clients like Copilot, ChatGPT, Gemini, or Claude. Those require uploads, and usually only let models “see” a snipped-down part of a file. It’s great for small tasks, but means prompts like “summarize this 30-page essay in 3 paragraphs” rarely work - the entire essay never actually reaches the model!

## Example: Save File Content
AIDA can save files to your computer - anytime, anywhere. Just ask AIDA to save something, and it’ll drop a `.txt` file in your Downloads folder, filled with your requested content!

![AIDA screenshot of saving file content]()

![screenshot of that content]()

All files are saved in your Downloads folder (works on both Windows and Linux).

## Example: Navigate Folder Structure
AIDA can also “open folders.” That means it can see what’s inside a folder (list files and subfolders) - and LLMs are clever enough to chain this together, exploring whole folder trees if you want!

Say you want AIDA to summarize every reference to “Lake Dora” in the `./trips` folder. If it finds a folder called `lake_trips` inside, it knows how to follow the path and keep looking, regardless of subfolders.

This lets your LLM truly explore your PC to accomplish more complex, multitiered tasks.

![AIDA screenshot of navigating folder]()

## Example: Save/Load Chat
You can save your chat history - your entire conversation - with a single command. No need for fancy cloud storage or logins: just run `save`, and AIDA saves a `.json` record of your chat right to your Downloads folder.

![screenshot of saving a chat history]()

To pick up where you left off, use the `load` command, point AIDA to the chat history file, and it’ll restore your conversation in full context.

## How to Install
AIDA is open source and 100% free! Installing it is super easy.

1. Install the [.NET 9.0 runtime](https://github.com/TimHanewich/AIDA/blob/master/changelog.md) (right side of the page) if you don't have it installed already.
2. Download the latest version of AIDA from [the changelog](https://github.com/TimHanewich/AIDA/blob/master/changelog.md)
3. Unpack the AIDA download (it will come in a `.zip` folder)
4. Run `AIDA.exe` (for Windows) or `AIDA` (for Linux)!

For more information visit [AIDA on GitHub](https://github.com/TimHanewich/AIDA)!