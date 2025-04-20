---
🧠 KaggleRAGent: a play on RAG + Agent + Kaggle - Research Assistant using Gemini
------

# 🧠 Bridging Research and Practice: A Gemini-Powered Agent for Kaggle Competitions Using RAG

## ✨ Introduction
The pace of machine learning research is accelerating rapidly. Every week, hundreds of new papers are published on ArXiv — yet only a fraction of those insights make it into the practical workflows of data scientists. Meanwhile, platforms like Kaggle host real-world challenges that would benefit tremendously from state-of-the-art techniques.

What if we could bridge the gap between research and real-world applications — automatically?

Meet RAGent: an intelligent agent powered by Google Gemini, built to connect cutting-edge ArXiv research papers with ongoing Kaggle competitions using a Retrieval-Augmented Generation (RAG) approach.

## 🎯 Goal of the Project
This project aims to:

Help data scientists discover and apply recent research papers relevant to ongoing Kaggle competitions.

Build an agent that reasons across multiple data sources: ArXiv (for research) and Kaggle (for competitions and leaderboards).

Use Gemini 2.0 Flash to generate concise, actionable insights by combining retrieved information using RAG chaining.

## 🧠 Agent Design Overview
We use the Google Generative AI SDK to create a multitool Gemini-powered agent, combining structured retrieval with smart prompting.

## 🔧 Tools Integrated
get_arxiv_papers(topic, count): Returns structured paper data from ArXiv (title, authors, summary, PDF URL, GitHub).

get_active_competitions(sort_string): Retrieves active Kaggle competitions based on deadline, prize, teams, etc.

get_leaderboard(ref): Fetches leaderboard data for any competition.

search_competitions_by_topic(topic): Searches Kaggle for relevant competitions.

## 🔄 RAG Flow
Retrieve:

Fetch papers from ArXiv using a keyword/topic.

Search Kaggle competitions based on the same topic.

Augment:

Combine the outputs into a structured prompt.

Pass this context into Gemini with a system instruction for JSON-based reasoning.

Generate:

Let Gemini reason across the context and respond with competition insights, paper summaries, and a final strategy.

