Sure, here is a comprehensive README file:

---

# README: Using LlamaParse with GPT-4o for Document Parsing and RAG Pipeline

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Installation](#installation)
4. [Usage](#usage)
    1. [Setup Environment](#setup-environment)
    2. [Parse the Document](#parse-the-document)
    3. [Build RAG Pipeline](#build-rag-pipeline)
5. [Notes](#notes)
6. [Conclusion](#conclusion)

## Introduction

LlamaParse is a document parsing library that supports the use of OpenAI's GPT-4o, a fully multimodal model with enhanced vision and audio capabilities. This guide demonstrates how to use LlamaParse with GPT-4o to parse a document and build a simple Retrieval-Augmented Generation (RAG) pipeline for querying the parsed data.

## Prerequisites

- Python 3.7 or higher
- LlamaParse library
- OpenAI GPT-4o access
- API keys for LlamaParse and GPT-4o (if applicable)
- Jupyter Notebook (optional, for running the example in an interactive environment)

## Installation

1. **Install Required Libraries:**
   ```bash
   pip install llama-parse nest-asyncio
   ```

2. **Download Example Document:**
   ```bash
   wget "https://www.dropbox.com/scl/fi/vu6w1dsfo5eddydz13ssm/2019-tesla-impact-report-15.pdf?rlkey=ik8lfqbg2p1ervss4qqt3xose&st=70j04z8j&dl=1" -O "2019-tesla-impact-report-15.pdf"
   ```

## Usage

### Setup Environment

1. **Import Required Libraries:**
   ```python
   import nest_asyncio
   import os
   from llama_parse import LlamaParse

   nest_asyncio.apply()
   os.environ["LLAMA_CLOUD_API_KEY"] = "<LLAMA_CLOUD_API_KEY>"
   ```

2. **Initialize LlamaParse with GPT-4o Mode:**
   ```python
   parser_gpt4o = LlamaParse(
       result_type="markdown",
       gpt4o_mode=True,
       split_by_page=True,
   )
   ```

### Parse the Document

1. **Load and Parse Document:**
   ```python
   documents_gpt4o = parser_gpt4o.load_data("./2019-tesla-impact-report-15.pdf")
   print(documents_gpt4o[3].get_content())
   ```

### Build RAG Pipeline

1. **Install Additional Libraries for RAG Pipeline:**
   ```bash
   pip install llama-index
   ```

2. **Build Vector Store and Query Engine:**
   ```python
   from llama_index.core import VectorStoreIndex

   vector_index = VectorStoreIndex(documents_gpt4o)
   query_engine = vector_index.as_query_engine(similarity_top_k=6)
   ```

3. **Query the Parsed Data:**
   ```python
   response = query_engine.query(
       "What are the greenhouse emissions for agriculture and transportation?"
   )
   print(str(response))
   ```

4. **Example Response:**
   ```
   Agriculture accounts for 20% of global greenhouse gas emissions, while transportation contributes 16% of these emissions.
   ```

5. **Query Another Piece of Text:**
   ```python
   response = query_engine.query(
       "How does the EPA range of Teslas compare with other vehicles? Give details"
   )
   print(str(response))
   ```

6. **Example Response:**
   ```
   The EPA range of Tesla vehicles varies across different models. The Model 3 Standard Range Plus (SR+) achieves an EPA range of 4.8 miles/kWh, making it the most efficient electric vehicle in production. The Model Y all-wheel drive (AWD) achieves 4.1 miles/kWh, which positions it as the most efficient electric SUV produced to date. The energy efficiency of Tesla vehicles is highlighted by these EPA range figures, showcasing their advancements in powertrain efficiency compared to other electric vehicles on the market.
   ```

## Notes

- The usage of GPT-4o with LlamaParse is more expensive, as each page parsed with GPT-4o counts as 10 pages in the LlamaParse usage tracker.
- You can provide a `gpt4o_api_key` to use your API key for GPT-4o calls, which will count each page as a single page instead of 10 pages.
- Ensure to handle API keys securely and not hard-code them in your scripts.

## Conclusion

This guide provided a walkthrough on how to use LlamaParse with GPT-4o for document parsing and building a simple RAG pipeline for querying parsed data. With GPT-4o's multimodal capabilities, you can perform advanced document extraction and build intelligent querying systems over parsed documents.

---

