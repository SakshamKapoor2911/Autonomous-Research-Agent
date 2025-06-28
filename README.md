# Autonomous Research Agent with RAG

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/LangChain-white?style=for-the-badge&logo=langchain&logoColor=black" alt="LangChain">
  <img src="https://img.shields.io/badge/Milvus-00A6A6?style=for-the-badge&logo=milvus&logoColor=white" alt="Milvus">
  <img src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-blue?style=for-the-badge" alt="Hugging Face">
</p>

## 🚀 Project Overview

This project features an autonomous research agent designed to tackle complex, high-level questions by breaking them down into a manageable research plan. The agent leverages a **Retrieval-Augmented Generation (RAG)** pipeline to systematically find answers, culminating in a synthesized, well-structured report.

For a given query like *"How has The Simpsons changed over time?"*, the agent autonomously performs the following actions:
1.  Decomposes the main query into a multi-level hierarchy of sub-questions.
2.  Builds a fresh, relevant knowledge base by fetching data from the Wikipedia API and embedding it into a **Milvus** vector store.
3.  Iteratively answers each sub-question using the RAG pipeline to ensure fact-based responses.
4.  Synthesizes the findings into a coherent, organized Markdown report.

This project showcases an end-to-end agentic workflow, from dynamic planning and tool use (RAG) to final content generation.

---

## 📋 Table of Contents
* [Key Features](#-key-features)
* [System Architecture](#️-system-architecture)
* [Tech Stack](#️-tech-stack)
* [Getting Started](#️-getting-started)
  * [Prerequisites](#prerequisites)
  * [Installation](#installation)
  * [Usage](#usage)
* [Example Output](#-example-output)
* [Future Work](#-future-work)

---

## ✨ Key Features

* **Autonomous Query Decomposition:** The agent intelligently breaks down a broad question into a hierarchical tree of specific, researchable sub-questions.
* **Dynamic RAG Pipeline:** Implements a Retrieval-Augmented Generation (RAG) system using **LangChain** and a **Milvus** vector database to provide contextually relevant, fact-based answers.
* **LLM-Powered Workflow:** Utilizes a large language model (`unsloth/DeepSeek-R1-Distill-Llama-8B-unsloth-bnb-4bit`) for all reasoning tasks, including query analysis, decomposition, and synthesis.
* **Hierarchical Content Synthesis:** Organizes the answers from the sub-questions into a structured and readable Markdown report, complete with corresponding headers.
* **Efficient Tooling:** Leverages **Unsloth** for memory-efficient model loading and **Sentence-Transformers** for creating high-quality embeddings.

## 🏗️ System Architecture

The agent follows a four-stage process to transform a high-level question into a detailed report.


+------------------+      +---------------------------+      +---------------------+      +----------------+      +----------------+
|   Initial Query  |----->|  Hierarchical             |----->|  Iterative          |----->|  Answer         |----->| Markdown       |
| (e.g., "Simpsons")|      |  Query Decomposition      |      |  RAG Answering      |      |  Synthesis      |      |  Report        |
+------------------+      +---------------------------+      +---------------------+      +----------------+      +----------------+


1.  **Query Decomposition:** The LLM receives the initial query and is prompted to break it down into a JSON structure of main questions and sub-questions.
2.  **Knowledge Base Creation:** The agent identifies the core subject, fetches the corresponding Wikipedia article, chunks the text, and embeds it into a Milvus vector store.
3.  **Iterative Answering:** For each sub-question, the agent retrieves the most relevant text chunks from Milvus and feeds them to the LLM as context to generate a focused answer.
4.  **Synthesis:** The agent assembles the questions (as headers) and the generated answers into a single, structured Markdown file.

## 🛠️ Tech Stack

* **Core Logic:** Python
* **LLM & Orchestration:** LangChain, Hugging Face Transformers
* **Efficient LLM Loading:** Unsloth
* **Vector Database:** Milvus (via `langchain_milvus`)
* **Embeddings:** Sentence-Transformers (`all-mpnet-base-v2`)
* **Data Source:** Wikipedia API
* **Utilities:** `json-repair`, `tqdm`

## ⚙️ Getting Started

### Prerequisites

* Python 3.10+
* `pip` package manager

### Installation

1.  Clone the repository:
    ```bash
    git clone [https://github.com/yourusername/your-repo-name.git](https://github.com/yourusername/your-repo-name.git)
    cd your-repo-name
    ```
2.  Create a `requirements.txt` file with the following contents:
    ```
    unsloth
    pymilvus
    wikipedia-api
    langchain
    langchain_huggingface
    langchain_core
    langchain_text_splitters
    langchain_community
    langchain_milvus
    sentence-transformers
    json-repair
    torch
    transformers
    tqdm
    bitsandbytes
    accelerate
    ```
3.  Install the required packages:
    ```bash
    pip install -r requirements.txt
    ```

### Usage
1. Open the Jupyter Notebook provided in the repository.
2. Locate the cell where the Wikipedia API is initialized:
    ```python
    wiki_wiki = wikipediaapi.Wikipedia(user_agent='MilvusDeepResearchBot (<insert your email>)', language='en')
    ```
   Replace `<insert your email>` with your own email address as per Wikipedia's API etiquette.
3. Run the cells of the notebook sequentially to execute the full research and synthesis pipeline. The final report will be saved as `report.md`.

## 📄 Example Output

For the query "How has The Simpsons changed over time?", the agent generates the following structured report:

> # The evolution of The Simpsons as a show over time, covering changes in content, humor, character development, animation, and its role in society.
>
> ## How has the content and themes of The Simpsons evolved?
> ### What were the initial content and themes of The Simpsons in its early seasons?
> In its early seasons, The Simpsons focused on the dysfunctional but loving Simpson family, using satire to comment on American life. The show's initial themes included family dynamics, school life, and the absurdities of suburban existence, often parodying pop culture and societal norms. The humor was characterized by its sharp wit, irony, and willingness to tackle controversial topics, which set it apart from other animated shows of the era.
>
> ### How did the animation style of The Simpsons change over the years?
> The animation style of The Simpsons has undergone significant changes since its debut. Initially, the show had a rougher, more inconsistent look, which was refined after the first season. Over the years, the animation became smoother and more polished with the adoption of digital coloring in the mid-1990s and a complete switch to digital animation in the 2000s. The transition to high-definition in 2009 further enhanced the visual quality, providing more vibrant colors and detailed backgrounds while maintaining the show's iconic aesthetic.
> *... and so on for all sub-questions.*

## 🔮 Future Work

* **Multi-Source RAG:** Expand the knowledge base beyond a single Wikipedia page by enabling the agent to perform web searches and pull information from multiple URLs.
* **Self-Correction & Validation:** Implement a validation step where the agent reviews its generated sub-questions and answers for coherence and relevance, re-prompting itself if the quality is low.
* **Advanced Retrieval Strategies:** Integrate more sophisticated retrieval techniques, such as hybrid search or re-ranking, to improve the context provided to the LLM.

<p align="right">(<a href="#-autonomous-research-agent-with-rag">back to top</a>)</p>
