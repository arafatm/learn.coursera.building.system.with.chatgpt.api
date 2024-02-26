---
created: 2024-02-25T18:07:42 (UTC -08:00)
tags: []
source: https://www.coursera.org/learn/building-systems-with-the-chatgpt-api-project/ungradedLti/K3PN1/building-systems-with-the-chatgpt-api
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

So I'll also share with you what the
process of developing an LLM-based application feels like, and some best
practices for evaluating and improving a system over time.
