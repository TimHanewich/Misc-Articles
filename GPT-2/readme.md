**THIS IS PUBLISHED ON MEDIUM: https://timhanewich.medium.com/running-openais-gpt-2-language-model-on-your-pc-5d5e1b9fbb8b**

# Running OpenAI's GPT-2 Language Model on your PC
OpenAI's ChatGPT has become incredibly popular lately due to its advanced language processing capabilities and ability to engage in natural and intelligent conversations with users. As one of the largest and most sophisticated language models available, ChatGPT is able to understand and respond to a wide range of topics, from simple queries to complex discussions about current events and scientific discoveries. With its unparalleled language processing abilities, ChatGPT has revolutionized the way people interact with technology and has quickly become a household name in the world of artificial intelligence.


## What is GPT?
**GPT** stands for **Generative Pre-trained Transformer**, a family of language models that use deep learning techniques to generate natural language text. To date, OpenAI has worked on several major models:
- GPT-1: Introduced in 2018, this was the first GPT model, with 117 million parameters.
- GPT-2: Released in 2019, this model had 1.5 billion parameters and was known for its ability to generate high-quality, coherent text.
- GPT-3: Launched in 2020, this is the most powerful GPT model yet, with 175 billion parameters. It can perform a wide range of natural language tasks, including language translation, question-answering, and text completion.

OpenAI's *ChatGPT* is an implementation of the GPT-3 model, fine-tuned to embody an assistant personality and generally be helpful by doing things like answering questions.

## GPT-2
Before ChatGPT, GPT-2 was OpenAI's first model that amazed the world with its ability to generate high-quality text. Its [story about unicorns](https://openai.com/blog/better-language-models/) was what first caught the attention of millions. 

While it isn't nearly as powerful as ChatGPT with only 0.8% of the computing power, GPT-2 is open-source, but getting the outdated code (and libraries) to run today can be challenging. Here is a step-by-step guide on how I did it:

## 1: Install Python
I tried multiple versions of Python, but the only version I found that works is version 3.7.0

Download Python 3.7.0 [here](https://www.python.org/downloads/release/python-370/)

## 2: Download the GPT-2 Repo
Use Git to clone the GPT-2 code from GitHub:
```
git clone https://github.com/openai/gpt-2
```

## 2: Create a virtual environment
Create your virtual environment:
```
python -m venv rungpt2
```

Activate your virtual environment (I'm using command prompt in Windows here)
```
rungpt2\Scripts\actiate
```

## 3: Install Packages
First, let's install tensorflow. The only version of tensorflow I found works with GPT-2 is [version 1.13.1](https://pypi.org/project/tensorflow/1.13.1/).
```
python -m pip install tensorflow==1.13.1
```
Install fire version 0.1.3:
```
python -m pip install fire==0.1.3
```
Install requests version 2.21.0
```
python -m pip install requests==2.21.0
```
Install tqdm version 4.31.1
```
python -m pip install tqdm==4.31.1
```

## 4: Downgrade Protobuf
There was some compatibility issue that arose with the version of Protobuf that was downloaded with TensorFlow version 1.13.1. *Downgrade* Protobuf to version 3.20.0 with the following command:
```
python -m pip install protobuf==3.20.0
```

## 5: Install regex
Install the latest version of regex. I used version 2022.10.31 in my specific case.
```
python -m pip install regex
```

## 6: Download models
With all of the dependencies installed in our virtual environment, we are now ready to download the model we would like to use. OpenAI released several different models that are compatible with GPT-2, each varying in size and computational power. The options available are:
- **117M**: This is the smallest version of the model, with 117 million parameters.
- **345M**: This is the medium-sized version of the model, with 345 million parameters.
- **774M**: This is the large version of the model, with 774 million parameters.
- **1558M**: This is the largest version of the model, with 1.5 billion parameters.


While the larger models produce higher-quality output, they also take significantly longer. Once you have your model picked out, run the following script from the `gpt-2` repo to download the model:

*(from within the directory gpt-2)*
```
python download_model.py 774M
```
You'll wait some time as the script downloads the model from Microsoft Azure to the `models` directory in your local `gpt-2` repo.

## 7: Run it!
You're now prepared to run GPT-2! From a command line, within the root directory of your `gpt-2` repo, run the following python script for an interactive experience with GPT-2:
```
python src/interactive_conditional_samples.py --model_name 774M --top_k 40 --length 256
```
Obviously replace "774M" with the model name that you chose to download in step 6. The `length` parameter controls how many "tokens" the model will be limited to when responding. I won't go into detail with the remainder of the parameters, but you'll find a description of each in the `interactive_conditional_samples.py` script:

[IMAGE HERE]

## 8: Custom Training (optional)
GPT-2 also allows you to train it on a corpus of text/other data. I will not go through that in this article but you can search the web and find some tutorials for doing so.

## 8: Prompting GPT-2
You cannot prompt GPT-2 like you would prompt ChatGPT. GPT-2 is purely a sentence completion engine. You enter in the beginnings of a sentence/though/paragraph and it will complete it for you. See the examples I provided below.

## Examples
