---
title: "ChatGPT for Novel Translation"
date: 2023-03-25
description: ChatGPT for novel translation
menu:
  sidebar:
    name: ChatGPT for Novel Translation
    identifier: chatgpt
    weight: 12
tags: ["chatgpt", "nlp"]
categories: ["nlp"]
---

ChatGPT is a powerful and versatile large language model that can assist in many NLP tasks. One such task is translation, and in this blog post, we will show you how to use ChatGPT to translate Chinese web novels into English.

## Step 1: Create an OpenAI account

Before you can use the ChatGPT model for novel translation, you need to create an OpenAI account to obtain an API key. This key will grant you access to the platform's powerful tools and resources, including the ChatGPT model.

To create an OpenAI account, go to [https://platform.openai.com/signup](https://platform.openai.com/signup) and follow the prompts to register. Once you have completed registration, you can view your API key by expanding the drop-down menu located at the top-right corner of the page. It is important to store your secret key in a secure location and not share it with anyone else.

{{< figure src="/posts/chatgpt-translation/api.png" height="400"  align="center" >}}

## Step 2: Install the OpenAI Python Package

With your API key in hand, you can now install the OpenAI Python package. This package will allow you to interface with the ChatGPT model and run translations on your local machine.

To install the package on your computer, open the terminal and type the following command:

`pip install openai`

This will download and install the package, which you can then import and use in your Python scripts.

## Step 3: Write Python Script

By following these two steps, you will be ready to use the ChatGPT model for novel translation. With its powerful language processing capabilities, you can quickly and accurately translate Chinese web novels into English. 

First, import the necessary packages:

```python
import requests, uuid, json
import argparse
# from cred import mykey, mylocation, chatkey
import math
import openai
import pdb 
import time
```

Then create a function `chatgpt_translate()`

```python
def chatgpt_translate(text_body):
    # Set up the API key, chatkey is the secret key you just saved.
    openai.api_key = chatkey

    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo", 
        messages=[
            {"role": "system", "content": "You are a helpful assistant that translates Chinese to English for a novel."},
            {"role": "user", "content": 'translate following Chinese to English: '+text_body}
        ]
    )

    # Extract the generated text from the response
    generated_text = response.choices[0].message.content.strip('Translation: \n')

    return generated_text
```

Define the main function that takes the novel in original language as input, and calls the chatgpt_translate() function

```python
def main(args):
    with open(args.input,'r') as f:
        data = f.readlines()

    data = [line for line in data if line !='\n']

    with open(args.output, 'w') as f:
        pass # flush content 

    num_requests = int(math.ceil(1. * len(data)/args.max_line) )
    for i in range(num_requests):
        text_list = data[i * args.max_line : min((i+1)*args.max_line, len(data))]
        
        text_body = '\n'.join(text_list)
        response = chatgpt_translate(text_body)
        

        with open(args.output, 'a') as f:
            f.write('\n\n')
            f.write(response)
            
        time.sleep(10)
```

Parse the arguments 

```python
if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='Argument Parser')
    parser.add_argument('--input', type=str, default = '../xiaoshuo/hei_wu_zhi_xia/chapter_0001.txt')
    parser.add_argument('--max_line', type = int, default = 50)
    parser.add_argument('--output', type=str, default = '../xiaoshuo/hei_wu_zhi_xia/translate_0001.txt')
    args = parser.parse_args()

    main(args)
```

## Step 4: Run the Python Script

If you save the above code in a file called `chatgpt-translate.py`, then you can use this script like this:

```python
python3 chatgpt-translate.py --input='../xiaoshuo/hei_wu_zhi_xia/chapter_0002.txt' --output='../xiaoshuo/hei_wu_zhi_xia/translate_0002.txt'
```

If you need more information or help with the process, visit the OpenAI documentation at [https://platform.openai.com/docs/api-reference/introduction](https://platform.openai.com/docs/api-reference/introduction).

Reference: [https://platform.openai.com/docs/api-reference/introduction](https://platform.openai.com/docs/api-reference/introduction)
