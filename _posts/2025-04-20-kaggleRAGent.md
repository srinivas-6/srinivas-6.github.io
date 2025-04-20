------

------

# 🧠 KaggleRAGent: Bridging Research and Practice, A Gemini-Powered Agent for Kaggle Competitions Using RAG

## ✨ Introduction
The pace of machine learning research is accelerating rapidly. Every week, hundreds of new papers are published on ArXiv — yet only a fraction of those insights make it into the practical workflows of data scientists. Meanwhile, platforms like Kaggle host real-world challenges that would benefit tremendously from state-of-the-art techniques.

What if we could automatically bridge the gap between research and real-world applications?

Meet KaggleRAGent: an intelligent agent powered by Google Gemini, built to connect cutting-edge ArXiv research papers with ongoing Kaggle competitions using a Retrieval-Augmented Generation (RAG) approach.

## 🎯 Goal of the Project
This project aims to:

* Help data scientists discover and apply recent research papers relevant to ongoing Kaggle competitions.

* Build an agent that reasons across multiple data sources: ArXiv (for research) and Kaggle (for competitions and leaderboards).

* Use Gemini 2.0 Flash to generate concise, actionable insights by combining retrieved information using RAG chaining.

## 🧠 Agent Design Overview
We use the Google Generative AI SDK to create a multitool Gemini-powered agent, combining structured retrieval with smart prompting.

![image](https://github.com/user-attachments/assets/871625f9-50ea-4a6a-baad-dce311a87954)

## 🔧 Tools Integrated
This agent operates through a tightly coordinated sequence of four core functions, each responsible for a specific stage in the information retrieval and reasoning pipeline. These functions aren't just arbitrary tools — they form a logical chain that reflects how a human expert would search, filter, and synthesize information from multiple sources like ArXiv and Kaggle. To ensure the model follows this flow correctly, we use **function_calling_config**, which explicitly defines the order and availability of functions at each step. This configuration empowers the agent to think more systematically, invoking only the relevant tools based on the user query and maintaining a coherent, context-aware workflow. It transforms the agent from a reactive fetcher into an intelligent planner that retrieves, aligns, and reasons across different data sources to deliver actionable insights.

* get_arxiv_papers(topic, count): Returns structured paper data from ArXiv (title, authors, summary, PDF URL, GitHub).

* get_active_competitions(sort_string): Retrieves active Kaggle competitions based on deadline, prize, teams, etc.

* get_leaderboard(ref): Fetches leaderboard data for any competition.

* search_competitions_by_topic(topic): Searches Kaggle for relevant competitions.

## 🛠️ Code Highlights
### Tool-Configurable Chat Session with Long-Term Context
With persistent chat sessions, the agent maintains context across queries, making interactions more coherent and goal-oriented.

```
chat = client.chats.create(
    model="gemini-2.0-flash",
    config=types.GenerateContentConfig(
        system_instruction=system_instruction,
        tools=[get_arxiv_papers, get_active_competitions, get_leaderboard, search_competitions_by_topic]
    ),
)

```
### MultiTool Request
The agent smartly combines outputs from multiple tools (ArXiv, Kaggle) in a single prompt, enabling richer, cross-domain reasoning.

```
response = client.models.generate_content(
    model="gemini-2.0-flash",
    contents="Search for competitions on LLMs and give me 5 top research papers about the same.",
    config=config
)

```

## 🔄 RAG Flow
### Retrieve:

* Fetch papers from ArXiv using a keyword/topic.

* Search Kaggle competitions based on the same topic.

### Augment:

* Combine the outputs into a structured prompt.

* Pass this context into Gemini with a system instruction for JSON-based reasoning.

### Generate:

Let Gemini reason across the context and respond with competition insights, paper summaries, and a final strategy.

## 💡 Why RAG Matters
### Without RAG:

* The agent just fetches individual results.

* Limited reasoning across tools.

### With RAG:

* Results are combined into structured prompts.

* Gemini reasons across sources, offering ranked, summarized, and strategic responses.

## 🔁 Zero-Shot vs Few-Shot Prompts
To evaluate the capabilities of the agent, I experimented with both zero-shot and few-shot prompting techniques. In the zero-shot setup, the model was given only the user query, relying entirely on its prior knowledge and reasoning ability. For few-shot prompting, I provided structured examples — including pairs of relevant competitions, research papers, and reasoning summaries — to guide the model's responses.

This approach helped me understand how context and example-driven prompting can significantly improve the relevance and accuracy of the agent's output, especially in aligning research papers with active Kaggle competitions.
### ✅ Zero-Shot Example
Zero-shot prompts are prompts that describe the request for the model directly. Here is an example of a zero shot prompt given as an input to the model.
```
Find the most recent 3 research papers on "Diffusion Models in Computer Vision" and list any active Kaggle competitions that may be related. Summarize how the papers could be used to get an advantage in those competitions.

```
### ✅ Few-Shot Example
Providing an example of the expected response is known as a "one-shot" prompt. When you provide multiple examples, it is a "few-shot" prompt. Here is an example prompt that expects the response in a JSON format.

```
### Example 1
User: Show me papers on "Self-Supervised Learning" and Kaggle challenges related to it.
Assistant:
{
  "competitions": [
    {"NAME": "Self-Supervised Vision Challenge", "URL": "...", "DESCRIPTION": "...", ...}
  ],
  "papers": [
    {"TITLE": "A Simple Framework for Contrastive Learning", "AUTHORS": "...", "URL": "...", ...}
  ],
  "final_summary": [
    "These papers explain how contrastive losses are used, which is relevant to the competition that requires feature extraction without labels..."
  ]
}

### Now your turn
User: Show me the latest research papers on "Diffusion Models in Computer Vision" and any related Kaggle competitions. How can I apply these ideas to gain an edge?

```

## 📓 Explore the Full Kaggle Notebook
👉 [Check out the complete implementation on Kaggle]([url](https://www.kaggle.com/code/srinivasravuri/kaggleragent))
It includes the agent code, RAG flow, Gemini integration, and visual walkthrough — along with zero-shot and few-shot prompt examples tested for topics like Visual SLAM, Diffusion Models, and LLM-based competitions.

## 📘 Final Thoughts
This project demonstrates how LLMs + Tool Use + RAG can supercharge the workflow of machine learning practitioners. Instead of reading 20 papers or browsing 100 competitions, you get curated, connected insights — instantly.

Future work includes:

* Adding more sources (Google Scholar, PapersWithCode)

* Incorporating user feedback loops

* Generating starter notebooks/code from papers

## 🌍 Hosting It on Hugging Face Spaces
I would like to work further and wrap this agent inside a Gradio interface and host it as an app on Hugging Face Spaces. Users can:

* Ask natural language queries.

* See papers and competitions side-by-side.

* Get an actionable summary on how to proceed.
  
<span style="color:green">**Stay tuned for the release!**</span>

## 📚 References

* [ArXiv API Documentation](https://info.arxiv.org/help/api/index.html)

* [Kaggle CLI Documentation](https://github.com/Kaggle/kaggle-api)
  
* [Gemini API – Google Generative AI](https://ai.google.dev/)

