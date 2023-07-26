+++
authors = ["tanmay"]
categories = ["AI"]
date = 2023-07-18T18:30:00Z
featured = true
gallery_item = []
lastmod = ""
projects = []
subtitle = "With Sentiment Analysis, Summarization and Categorization"
summary = "An app that fetches top news articles using an API, generates summaries of each using the OpenAI GPT-3, analyzes sentiment using the NLTK library, and classifies them into different categories."
tags = ["GPT3", "NLKT", "API", "Python"]
title = "Making sense of the news with GPT-3, NLTK, Python and Streamlit"
[image]
caption = "App Screenshot by Tanmay"
focal_point = "smart"
preview_only = false

+++
ChatGPT buzz is all around and I was keen to use the GPT-3 API in a project. In this post I will walk through the process of creating a Python web app using Streamlit, called "News 📰 Sense." The app fetches top news articles from the News API, generate summaries of each article using the OpenAI GPT-3 model, analyze the sentiment of the article using the Natural Language Toolkit (NLTK) library, and classifies the articles into different categories based on keywords.

## Understanding Sentiment Analysis
Sentiment analysis is a natural language processing (NLP) technique used to determine the sentiment or emotion expressed in a piece of text. It aims to understand whether the text conveys a positive, negative, or neutral sentiment. For my project I used the  [VADER](https://www.nltk.org/_modules/nltk/sentiment/vader.html)  (Valence Aware Dictionary and sEntiment Reasoner) lexicon from the NLTK library.

The VADER lexicon contains a list of words and their associated sentiment scores. It takes into account the intensity of the sentiment words and the context in which they appear to provide a compound sentiment score. A positive (pos) score indicates a positive sentiment, a negative (neg) score indicates a negative sentiment, and a score close to zero suggests a neutral (neu) sentiment.

## Categorizing News Articles
Categorizing news articles helps in organizing and presenting information effectively. In my app, I classify each article into predefined categories, such as Business, Technology, Sports, Entertainment, Politics, and Society. To achieve this, I created a list of keywords associated with each category. In the code I checked if any of these keywords are present in the article content, and if so, assigned the article to the corresponding category.

## Summarizing News Articles with GPT-3
Summarization is the process of condensing a longer piece of text into a shorter, concise version that captures the essential information. For summarizing news articles, I used the OpenAI  [GPT-3 DaVinci](https://platform.openai.com/docs/models/gpt-3), a powerful language generation model that can generate human-like text. Shameless admission: Using GPT was the main draw for this project :)

GPT-3 works by taking a prompt (the news article content in our case) and generating a continuation or summary based on that prompt. We specify the maximum number of tokens for the summary to control its length. The GPT-3 model is fine-tuned to understand context and generate coherent and contextually relevant summaries.

## The application
You can find the code for News 📰 Sense and instructions to use it at  [https://github.com/tanmaychk/News-Sense](https://github.com/tanmaychk/News-Sense). Before starting, make sure you have the following:
-   Python installed on your system    
-   An API key from the News API. You can sign up and get your API key from  [here](https://newsapi.org/).    
-   An API key from OpenAI GPT-3. You can obtain your API key from  [OpenAI](https://platform.openai.com/account/api-keys).    

## What have been the Learnings
As a developer, this project allowed me to apply various NLP concepts and technologies in a practical and meaningful way:
-   **API Integration**: how to integrate external APIs like the News API and OpenAI GPT-3 API into Python code to access real-time news data and perform advanced language generation.    
-   **Natural Language Processing (NLP)**: By utilizing the NLTK library, gained insights into how NLP techniques like sentiment analysis and tokenization work. I also understood how the VADER lexicon is used to determine the sentiment of text.    
-   **Machine Learning for Language Generation**: experimented with OpenAI GPT-3, a state-of-the-art language model capable of generating human-like text. I utilized it to summarize news articles, demonstrating the power of machine learning in language tasks.    
-   **Text Classification**: applied text classification to categorize news articles based on specific keywords. This demonstrated how NLP techniques can be used for content organization and user-friendly presentation.    
-   **Streamlit Web App Development**: I had used Streamlit earlier as well, in another project. Streamlit's simplicity allows us to focus on functionality and visualization rather than dealing with web development complexities.
    
## Further Steps
The News Sense app can perhaps be further enhanced by incorporating user input for customized news filtering, improving summarization techniques, or integrating more advanced language models. I will implement them, when I get time.

It was a joy working on the project, hope you liked it as well.

> Cross posted at [Dev.To](https://dev.to/tanmaychk/making-sense-of-the-news-with-gpt-python-and-streamlit-33c6)
