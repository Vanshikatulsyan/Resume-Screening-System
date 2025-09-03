# Resume Screening

## Introduction

An LLM chatbot that can assist hiring managers in the resume screening process. The assistant is a cost-efficient, user-friendly, and more effective alternative to the conventional keyword-based screening methods. Powered by state-of-the-art LLMs, it can handle unstructured and complex natural language data in job descriptions/resumes while performing high-level tasks as effectively as a human recruiter.  

The core design of the assistant involves the use of hybrid retrieval methods to augment the LLM agent with suitable resumes as context:

1. Adaptive Retrieval:
   - Similarity-based retrieval: When a job description is provided, the retriever utilizes RAG/RAG Fusion to search for similar resumes to narrow the pool of applicants to the most relevant profiles.
   - Keyword-based retrieval: When applicant information is provided (IDs), the retriever can also retrieve additional information about specified candidates.
3. Generation: The retrieved resumes are then used to augment the LLM generator so it is conditioned on the data of the retrieved applicants. The generator can then be used for further downstream tasks like cross-comparisons, analysis, summarization, or decision-making.


#### Why resume screening?

Despite the increasingly large volume of applicants each year, there are limited tools that can assist the screening process effectively and reliably. Existing methods often revolve around keyword-based approaches, which cannot accurately handle the complexity of natural language in human-written documents. Because of this, there is a clear opportunity to integrate LLM-based methods into this domain, which the project aims to address. 


## System Description

### 1. Chatbot Structure

The deployed chatbot utilizes certain techniques to be more suitable for real-world use cases:

- Chat history access: The LLM is fed with the entire conversation and the (latest) retrieved documents for every message, allowing it to perform follow-up tasks. 
- Query classification: Utilizing function-calling and an adaptive approach, the LLM extracts the necessary information to decide whether to toggle the retrieval process on/off. In other words, the system only performs document retrieval when a suitable input query is provided; otherwise, it will only utilize the chat history to answer.
- Small-to-Big retrieval: The retrieval process is performed using text chunks for efficiency. The retrieved chunks are then traced back to their original full-text documents to augment the LLM generator, allowing the generator to receive the complete context of the resumes. 

**Tech stacks:** 
- `langchain`, `openai`, `huggingface`: RAG pipeline and chatbot construction.
- `faiss`: Vector indexing and similarity retrieval.
- `streamlit`: User interface development.

### 2. Under-the-hood RAG Pipeline 

The pipeline begins by processing resumes into a vector storage. Upon receiving the input job descriptions query, the LLM agent is prompted to generate sub-queries. The vector storage then performs a retrieval process for each given query to return the top-K most similar documents. The document list for each sub-query is then combined and re-ranked into a new list, representing the most similar documents to the original job description. The LLM then utilizes the retrieved applicants' information as context to form accurate, relevant, and informative responses to assist hiring managers in matching resumes with job descriptions.

## Installation and Setup

To set up the project locally:
```
# Clone the project
git clone https://github.com/Jiya873/Screening-System.git

# Install dependencies
pip install requirements.txt
```

To run the Streamlit demo locally:
```
streamlit run demo/interface.py
```
