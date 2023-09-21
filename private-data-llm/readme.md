# How to Use Private Data with Large Language Models
With the massive rise of large language models in 2023, so many "conversation-based" services have spawned, giving users the ability to interact with data and other products through natural conversation. We now find ourselves in an era in which LLMs have transformed the way we interact with all kinds of data and information. Gone are the days of sifting through engless search results or deciphering complex user interfaces; now, all you need is natural language to begin exploring.

If you're considering creating your own chatbot service using a large language model as a service like OpenAI's GPT models, you may be curious how you can weave your *private* data into the LLM's "knowledge base". I'll explain this here:

## Training?
There are two fundamental ways in which an LLM can be "knowledgeable" of any data, using its understanding of the data to answer questions from the user. 

The first way, through **training**, is how the LLM established a foundational understanding of the world we live in. By exposing an LLM to millions upon millions of wikipedia articles, news articles, books, online forums, and more, these LLMs eventually gained the ability to construct meaningful sentences simply by predicting which word ("token") comes next in a sentence. During the training process, the LLM not only became capable of constructing complex sentences and paragraphs, it also became capable of explicitly using the knowledge it was was exposed to in its training material. This is why, despite having only been trained on a corpus of text from mid-2021 and before, ChatGPT is capable of divulging foundational knowledge of our understanding of the world we live in; it can answer questions like "why is the sky blue?" and "who was the first person to step foot on the moon?".

Through the tools provided to us by OpenAI, we can train a model ourselves - not from absolute scratch, but more "fine-tuning". If you were to train a model on a series of questions and answers that include the use of your private data, the model will absolutely capture your data and be able to use this later during conversations. 

However, *training* is usually not the solution for using private data in an LLM. Training is costly and requires meticulous planning, preparation, and processing time to accomplish. With data changing rapidly and the LLM needing to *always* use the most up-to-date version of certain data points, it would be impractical to train the model at a frequency high enough to always have the "latest" version of the data. And even if this were the case, there would not be a guarentee that the *correct* version of certain datapoints have truly "overwritten" the old versions in the LLM's "memory".

## Prompt Engineering
Instead, a far more intuitive solution to this problem is commonly used: prompt engineering. Instead of relying on model training, **prompt engineering is the process of asking a question of an LLM while simultaneously providing it with the context (data) it needs to answer the question**.

For example, take the three situations below:
![prompting](https://i.imgur.com/JSupDwV.jpg)

In the first question to the LLM, on the left, the user is asking for the date of birth for Bill Gates, the co-founder of Microsoft. Bill Gates is a very famous figure and has many wikipedia pages and internet articles written on him and his inventions. During the training process that OpenAI designed and executed to train their GPT models, the model was undoubteble exposed to and trained on articles about Bill Gates. Thus, the model "learned" Bill Gates' birthday and "stored it in memory", allowing it to recall this information when asked, as shown.

In the second question to the LLM, in the middle, the LLM is asked when Daniel Smith was born. There is no famous figure named Daniel Smith that this GPT model had been trained on previously, thus no "knowledge" of Daniel Smith or his birthday lives within the "memory" of the LLM, and the LLM answers as such.

However, in the third question to the LLM, on the right, we are asking the same question - "When was Daniel Smith born?", but also providing the LLM with the required **context** (*data*) that it needs to answer this question. 

This third example is known as *prompt engineering*. In this method, two pieces of information are always sent to the LLM as a prompt - the question itself, either following or followed by some context that is required to answer the question. While the two are not separated by anything more than a simple blank line, engineers often refer to the two pieces of information as different components: the *system prompt* and *user prompt*. The user prompt is the prompt that the user defined - the question. The system prompt is the information that was gathered and will be included with the user prompt as context when prompting the LLM. The system prompt is either prepended or appended to the user prompt and the result - both together - is then asked of the LLM.

## Constructing a System Prompt
In *prompt engineering*, the **system prompt** is where you would include any private data that you would want the LLM to use in answering a question. That *private data* can be anything from knowledge about processes for how a department should handle certain requests, instructions for how the LLM should respond or behave, explicit data points, or more. 

But the system prompt cannot be of unlimited size; LLMs like OpenAI's GPT series have limits on the amount of text they are capable of comprehend and answer. So you can't just dump every record of a database and allow the LLM to figure it out, unfortunately!

Instead, engineers spend countless hours developing systems to identify the data that they think will be most pertinent and helpful for a particular question, seeking that data out, and preparing it to be used in a system prompt. From **Bing AI**, **Google Search with AI**, **Power Virtual Agents**, **Power Apps Copilot**, and others, all of these systems use the same methodology.

In a way, this mechanism that the engineers design to pinpoint pertinent data, prepare this data, and include in a system prompt is the *secret sauce* and primary value driver in each of these services; the rest is just a simple API call to an LLM!

Thus, **to use private data with an LLM (construct chatbot-like experiences that use your private data), you must develop a mechanism that will search for relevant data, read that data, prepare that data, and append that data to the user prompt with some instructions, and then prompt an LLM service via an API call**. We will explore what a portion of this looks like below:

## Data Hummanization
As mentioned above, data must be selected and prepared to be included in a prompt to an LLM. While LLMs are very capable of interpreting and deciphering meaning from a jumbled mess, it is in our best interest to *transform* the data to a state that is more explicitly meaningful, allowing the LLM to better "understand" the data and use this in its response (avoid hallucinations, bad responses, etc).

Take for example, a raw data payload that looks like this (in JSON):
```
{
    "jf45ds_first": "John",
    "jf45ds_last": "O'Connor",
    "jf45ds_role": 857259845,
    "jf45ds_father": "ac7f2f16-466f-44a0-834f-4439f36ab41f",
    "jf45ds_mother": "f865d36f-a59a-4277-8291-90b719d18a7e",
    "jf45ds_gen": "m"
}
```
In the above data payload, we can surely make some educated guesses about this:
- It describes a person or contact record.
- This person's first name is **John**, their last name is **O'Connor**.
- Their "role" is whatever **857259845** represents. Perhaps this is their occupation? Security role?
- Data about this person's father is in a related record with ID **ac7f2f16-466f-44a0-834f-4439f36ab41f**.
- Data about this person's mother is in a related record with ID **f865d36f-a59a-4277-8291-90b719d18a7e**.

While we can make these educated guesses about this data, and the LLM can too, it is best practice to transform this payload to a far more legible and understandable version of itself. By using metadata about this table (each column's title, description, etc), we can *hummanize* this record:

```
{
    "First Name": "John",
    "Last Name": "O'Connor",
    "Occupation": "General Contractor",
    "Father": {"First Name": "James", "Last Name": "O'Connor"},
    "Mother": {"First Name": "Lynda", "Last Name": "O'Connor"},
    "Gender": "Male"
}
```

The "hummanized" payload above is much easier to understand and draw conclusions from. By providing this to the LLM, the LLM is able to extract much more meaning out of this data. By **appending this hummanized-version of this private data record to a user question**, the LLM can now answer questions about John such as:

- What is John's occupation?
- What is John's father's name?
- What is John's mother's name?
- What is John's gender?

So, a prompt to any LLM would look something like this:

```
Use the record below to answer the subsequent question:
{
    "First Name": "John",
    "Last Name": "O'Connor",
    "Occupation": "General Contractor",
    "Father": {"First Name": "James", "Last Name": "O'Connor"},
    "Mother": {"First Name": "Lynda", "Last Name": "O'Connor"},
    "Gender": "Male"
}

What is John's occupation?
```

In the example above, we are providing the hummanized record with explicit instructions before it, and prepending this to the user's question. We prompt an LLM with this and receive the response "General Contractor". Had we *only* prompted the LLM with the question "What is John's Occupation?", it would not know who John is!

## Is my Data Secure?
Obviously, in the process above, we are providing private data, along with a question, directly to the LLM. Is this safe? Is it secure?

Firstly, don't think that you must provide *all* private data to an LLM for any particular question. As mentioned previously, it is your responsibility to build the mechanism that prepares private data to be fed to an LLM as context to a question. Within this mechanism, build safeguards that prevent specific pieces of data that you don't wish to ever be exposed to any third party system from being used and sent to an LLM. For example, you may design your mechanism to only pull from specific tables but not others, to never include certain fields (i.e. social security numbers) in the payload that will be exposed, and more.  In developing a custom system like this, you have **complete control** over what is exposed to the LLM because **you are designing the mechanism that peforms the exposing**!

Secondly, this is a common concern that is shared by many that has been addressed by the stewards of the LLMs. Microsoft provides large language models as a service through its **Azure OpenAI service** and clearly documents the data privacy and security safeguards in place [here](https://learn.microsoft.com/en-us/legal/cognitive-services/openai/data-privacy). Microsoft clearly states any exposed data:

- is NOT available to other customers.
- is NOT available to OpenAI.
- is NOT used to improve OpenAI models.
- is NOT used to improve any Microsoft or 3rd party products or services.
- is NOT used for automatically improving Azure OpenAI models for your use in your resource (The models are stateless, unless you explicitly fine-tune models with your training data).

The documention Microsoft provides goes on at length about the safeguards in place to secure your private prompts (which include data you've included). Thus, exposing private data to LLMs on a secure and trusted platform like Microsoft Azure is safe.











- "conversation with data" so big in 2023
- 2 ways LLM's have "knowledge": training, prompt engineering
- training can be used for "tone" - see examples I made (gilligan, spongebob)
- Example prompt engineering w/ and w/o context
- Transforming ("hummanizing") data
- Using private data is secure w/ Azure OpenAI