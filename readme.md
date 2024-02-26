---
created: 2024-02-25T18:07:42 (UTC -08:00)
tags: ai, chatgpt, openai, api
source: https://learn.deeplearning.ai/chatgpt-building-system/
author: Andrew Ng
title: Building Systems with the ChatGPT API | Coursera
---

In **Building Systems With The ChatGPT API,** you will learn how to automate
complex workflows using chain calls to a large language model. Unlock new
development capabilities and improve your efficiency in this brand new short
course.

You’ll build:
-   Chains of prompts that interact with the completions of prior prompts.
-   Systems where Python code interacts with both completions and new prompts.
-   A customer service chatbot using all the techniques from this course.
    

You’ll learn how to apply these skills to practical scenarios, including
classifying user queries to a chat agent’s response, evaluating user queries
for safety, and processing tasks for chain-of-thought, multi-step reasoning. 

This one-hour course, taught by Isa Fulford (OpenAI) and Andrew Ng
(DeepLearning.AI), builds on the lessons taught in the popular [**ChatGPT
Prompt Engineering for Developers**Opens in a new
tab](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/),
though it is not a prerequisite. 

Hands-on examples make each concept easy to understand. Built-in Jupyter
notebooks allow you to seamlessly experiment with the code and prompts
presented in the course.

## 01 Introduction

### Transcript

Best practices for building a complex application using an LLM. 

So for example, given a user input like, "Tell me about what TVs are for
sale?", we'd use the following steps to process this. 
- First, you can evaluate the input to make sure it doesn't contain any
  problematic content, such as hateful speech. 
- Next, the system will process the input. It will identify what type of query
  this is. Is it a complaint or a product information request and so on? 
- Once it has established that it is a product inquiry, it will retrieve the
  relevant information about TVs and use a language model to write a helpful
  response.
- Finally, you'll check the output to make sure it isn't problematic, such as
  inaccurate or inappropriate answers.
 
One theme you see throughout this course is that an application often needs
**multiple internal steps** that are invisible to the end-user.

You often want to sequentially process the user input in multiple steps to get
to the final output that is then shown to the user.

And as you build complex systems using LLMs, over the long term you often want
to also keep on improving the system. 

So I'll also share with you what the process of developing an LLM-based
application feels like, and some best practices for evaluating and improving a
system over time.

## Language Models, the Chat Format and Tokens  

In this first video, I'd like to share with you an overview of how LLMs, Large
Language Models, work. We'll go into how they are trained, as well as details
like the tokenizer and how that can affect the output of when you prompt an
LLM.

The main tool used to train an LLM is actually supervised learning.

In supervised learning, a computer learns an input-output or X or Y mapping
using labeled training data. So for example, if you're using supervised
learning to learn to classify the sentiment of restaurant reviews, you might
collect a training set like this

| Input x                               | Output y |
| --                                    | --       |
| The pastrami sandwich is great!       | Positive |
| Service was slow, the food was so-so. | Negative |
| The earl grey tea was fantastic       | Positive |
| Best pizza I've ever had              | Positive |

Specifically, a *Large Language Model can be built by using supervised learning
to repeatedly predict the next word.*

Given "My favorite food is a bagel with cream cheese and lox." the sentence is
turned into a sequence of training examples, where given a sentence fragment, 

| Input x                          | Output y |
| --                               | --       |
| My favorite food is a            | bagel    |
| My favorite food is a bagel      | with     |
| My favorite food is a bagel with | cream    |

And given a large training set of hundreds of billions or sometimes even more
words, you can then create a massive training set where you can start off with
part of a sentence or part of a piece of text and repeatedly ask the language
model to learn to predict what is the next word.

So today there are broadly two major types of Large Language Models. 
- The first is a `Base LLM` and 
- the second, which is what is increasingly used, is the `Instruction Tuned LLM`.

### Base LLM

> Predicts next word, based on text training data

Given input: "Once upon a time there was a unicorn"
- Then it may, by repeatedly predicting one word at a time, come up with a
  completion that tells a story about a unicorn living in a magical forest with
  all her unicorn friends.
 
**Downside** of this is that if you were to prompt it with "What is the
capital of France?", quite possible that on the internet there might be a list
of quiz questions about France. So it may complete this with "What is France's
largest city, what is France's population?", and so on.

But __what you really want is you want__ it to tell you what is the capital of
France, probably, rather than list all these questions.

### Instruction Tuned LLM 

> Instead tries to follow instructions

Will hopefully say, "The capital of France is Paris. ". 

### Base -> Instruction Tuned

Steps:
1. You first train a Base LLM on a lot of data, so hundreds of billions of
   words, maybe even more. And this is a _process that can take months_ on a
   large supercomputing system.
2. Further train the model by fine-tuning it on a smaller set of examples,
   where the _output follows an input instruction_. And so, for example, you
   may have contractors help you write a lot of examples of an instruction, and
   then a good response to an instruction.
3. And that creates a training set to carry out this additional fine-tuning. So
   that learns to predict what is the next word if it's trying to follow an
   instruction. After that, to improve the quality of the LLM's output, a
   _common process now is to obtain human ratings_ of the quality of many
   different LLM outputs on criteria, such as whether the output is helpful,
   honest, and harmless.
4. And you can then further tune the LLM to increase the probability of its
   generating the more highly rated outputs. And the _most common technique to
   do this is RLHF_, which stands for Reinforcement Learning from Human
   Feedback. And whereas training the Base LLM can take months, the process of
   going from the Base LLM to the Instruction Tuned LLM can be done in maybe
   days on a much more modest size data sets, and much more modest size
   computational resources.

 
### Jupyter Notebook

[Jupyter Notebook](l1.ipynb)

:warning: Note in the example where we reverse "lollipop", we get an incorrect answer.

It turns out that there's one more important detail for how a Large Language
Model works, which is it doesn't actually repeatedly predict the next word, it
__instead repeatedly predicts the next token__.
 
And what an LLM actually does is it will take a sequence of characters, like
"Learning new things is fun!", and group the characters together to form tokens
that comprise commonly occurring sequences of characters.

So here, learning new things is fun, each of them is a fairly common word, and
so each token corresponds to one word, or one word in a space, or an
exclamation mark.

But if you were to give it input with some somewhat less frequently used words,
like "Prompting as powerful developer tool. ", the word prompting is still not
that common in the English language, but certainly gaining in popularity.

And so prompting is actually broken down to three tokens with "'prom", "pt",
and "ing", because those three are commonly occurring sequences of letters.

And if you were to give it the word lollipop, the tokenizer actually breaks
this down into three tokens, "l" and "oll" and "ipop".

And because ChatGPT isn't seeing the individual letters, is instead seeing
these three tokens, it's more difficult for it to correctly print out these
letters in reverse order.
 
__So here's a trick you can use to fix this__.

If I were to add dashes to the word dashes, between these letters, and spaces
would work too, or other things would work too, and tell it to take the letters
and lollipop and reverse them, then it actually does a much better job, this
L-O-L-L-I-P-O-P.

And the reason for that is, if you pass it lollipop with dashes in between the
letters, it tokenizes each of these characters into an individual token, making
it easier for it to see the individual letters and print them out in reverse
order.

So if you ever want to use ChatGPT to play a word game, like word or scrabble
or something, this nifty trick _helps it to better see the individual letters of
the words_.
 
For the English language, __one token roughly on average__, _corresponds to about
four characters or about three quarters of a word_.

And so different Large Language Models will often have different limits on the
number of input plus output tokens it can accept. 
- The input is often called the `context`, and 
- the output is often called the `completion`.

And the model GPT 3. 5 Turbo, for example, the most commonly used chat GPT
model, has a limit of roughly 4,000 tokens in the input plus output.

So if you try to feed it an input context that's much longer than this, it'll
actually throw an exception or generate an error. 

### System, User, Assistant

Next, I want to share with you another powerful way to use an LLM API. Which
involves _specifying separate system, user, and assistant messages_.

```python
messages =  [  
{'role':'system', 
 'content': "You are an assistant..."},    
{'role':'user', 
 'content': "Tell me a joke"},
{'role':'assistant', 
 'content': "Why did the chicken..."},
]
```

- `system` sets behavior of assistand
- `assistant` is the chat model
- `user` is you

Look at helper function called `get_completion_from_messages`, and when we
prompt this LLM, we are going to give it multiple messages.

Here's an example of what you can do. I'm going to specify first a message in
the role of a `system`, so this is a system message, and the content of the
system message is "You are an assistant who responds in the style of Dr. Seuss. " 

```python
messages =  [  
{'role':'system', 
 'content':"""You are an assistant who responds in the style of Dr Seuss."""},    
{'role':'user', 
 'content':"""write me a very short poem about a happy carrot"""},  
] 
response = get_completion_from_messages(messages, temperature=1)
print(response)
```

Then I'm going to specify a user message, so the role of the second message is
"role : user", and the content of this is "write me a very short poem about a
happy carrot".

And so let's run that, and with "temperature = 1", I actually never know what's
going to come out, but okay, that's a cool poem. "Oh, how jolly is this carrot
that I see?". 
 
And it actually rhymes pretty well. All right, well done ChatGPT. And so in
this example, the system message specifies the overall tone of what you want
the Large Language Model to do, and the user message is a specific instruction
that you wanted to carry out given this higher level behavior that was
specified in the system message.

Here's an illustration of how it all works.
- `system` sets tone/behavior of assistant
- `assistant` returns the LLM response
- `user` sets the prompts

See code for a few more examples.

If you want to set the tone, to tell it to have a one sentence long output,
then in the system message, I can say all your responses must be one sentence
long.

## Token count

To view prompt, completion, and total tokens see the `token_dict` return

## Tip: Use dotenv

## Prompting is revolutionizing AI development

Lastly, I think the degree to which prompting is revolutionizing AI application
development is still underappreciated.

In the traditional supervised machine learning workflow, like the restaurant
review sentiment classification example that I touched on just now, if you want
to build a classifier to classify restaurant review positive and negative
sentiments, you at first get a bunch of label data, maybe hundreds of examples.
This might take, I don't know, weeks, maybe a month.

Then you would _train a model on data_ and _getting an appropriate open source
model_, _tuning on the model_, _evaluating it_.

__That might take days, weeks, maybe even a few months.__

And then you might have to find a cloud service to deploy it, and then get your
model uploaded to the cloud, and then run the model, and finally be able to
call your model.

And it's again not uncommon for this to take a team a few months to get
working.

In contrast with prompting-based machine learning, when you have a text
application, you can specify a prompt.

This can take minutes, maybe hours, if you need to iterate a few times to get
an effective prompt. And then in hours, maybe at most days, but frankly more
often hours, you can have this running using API calls and start making calls
to the model. And once you've done that, in just again, maybe minutes or hours,
you can start calling the model and start making inferences. And so there are
applications that used to take me maybe six months or a year to build, that you
can now build in minutes or hours, maybe very small numbers of days using
prompting. And this is revolutionizing what AI applications can be built
quickly.

One important caveat, this applies to many unstructured data applications,
including specifically text applications and maybe increasingly vision
applications, although the vision technology is much less mature right now, but
it's kind of getting there.

This recipe doesn't really work for structured data applications, meaning
machine learning applications on tabular data with lots of numerical values in
Excel spreadsheets.

But for applications to which this does apply, the fact that AI components can
be built so quickly, is changing the workflow of how the entire system might be
built. Building an entire system might still take days or weeks or something,
but at least this piece of it can be done much faster.

And so with that, let's go on to the next video, where Isa will show how to use
these components to evaluate the input to a customer service assistant.

And this will be part of a bigger example that you see developed through this
course, for building a customer service assistant for an online retailer.

