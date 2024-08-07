# RAG with Data Prep Kit

This folder has examples of RAG applications with data prep kit.

These examples are designed to run on a local python dev environment.  See [setup-python-dev.md](../setup-python-dev-env.md) for instructions.

[Overview of RAG process](#rag-process)

## Code Matrix

Here is an easy to navigate matrix of various RAG query examples.  

You can easily find the code the uses the combination you are looking for.

### Example 1: Granite Documents PDF

#### Ex 1.1 - Ingestion using Data Prep Kit

`PDF --> Process with: Data Prep Kit --> Milvus --> query with: Llama LLM on replicate`

- [Clean up and processing documents](rag_1_A_dataprepkit_process_data.ipynb) - TODO
- [Loading data into Milvus](rag_1_B_dataprepkit_load_data_into_milvus.ipynb)
- [Query using llama LLM @ Replicate](rag_1_C_dataprepkit_query_llama_replicate.ipynb)

#### Ex 1.2 - Ingestion and query using Llama-Index

`PDF --> Process with: llama-index --> Milvus --> query with: llama-index --> Llama LLM on replicate`

- [Load and query data with Llama-index](rag_2_llamaindex_milvus_llama_replicate.ipynb)

### Example 2 : Walmart documents (PDF)

---


## Handy Utilities

[Inspect parquet files](./utils_inspect_parquet.ipynb) - Handy for seeing the contents of parquet files

[Vector search](./vector_search.ipynb) - do a quick vector search of any collection in Milvus DB


## RAG Process

RAG conists of two phases

- **1 - Import phase**:  Preparing and indexing the data  (the top row in the diagram)
- **2 - Querying phase**: query the data prepared in phase 1  (bottom row in the  diagram)

We will be using [IBM's data prep kit](https://github.com/IBM/data-prep-kit) to process data for RAG.  This includes steps (1), (2), (3) and (4)

![](../media/rag-overview-2.png)


### Step 1 (Import): Cleanup documents

Remove markups, perform de-duplication ..etc

[Code below](#processing-code)

### Step 2 (Import): Split into chunks

Split the documents into manageable chunks or segments. There are various chunking stratergies.  Documents can be split into pages or paragraphs or sections.  The right chunking strategy depends on the document types being processed

[Code below](#processing-code)


### Step 3 (Import): Vectorize / Calculate Embeddings

In order to make text searchable, we need to 'vectorize' them.  This is done by using **embedding models**.  We will feature a variety of embedding models, open source ones and API based ones.

[Code below](#processing-code)


### Processing Code

**Here is the code that performs steps 1, 2 and 3 : [code](rag_1_A_dataprepkit_process_data.ipynb)**

### Step 4 (Import). Saving Data into Vector Database

In order to effectivly retrieve relevant documents, we use [Milvus](https://milvus.io/) - a very popular open source, vector database.

[code](rag_1_B_dataprepkit_load_data_into_milvus.ipynb)

### Step 5 (Query). Vectorize Question 

When user asks a question, we are going to vectorize the question so we can fetch documents that **may** have the answer question.

For example, if a user asks a financial question ('how much was the revenue in 2022') the answer is most likely in financial documents, not in employee handbooks.

So we want to retrieve the relevant documents first.

[Code below](#rag-query-code)


### Step 6 (Query): Vector Search

We send the 'vectorized query' to vector database to retrieve the relevant documents.

[Code below](#rag-query-code)

### Step 7 (Query): Retrieve Relevant Documents

Vector database looks at our query (in vectorized form), searches through the documents and returns the documents matching the query.

This is an important step, because it **cuts down the 'search space'**.  For example, if have 1000 pdf documents, and only a small number of documents might contain the answer, it returns the relevant documents.

The search has to be accurate, as these are the documents sent to LLM as **'context'**.  LLM will look through these documents searching for the answer to our question

[Code below](#rag-query-code)

### Step 8 (Query): Send relevant documents and query LLM

We send the relevant documents (returned in the above step by Vector DB) and our query to LLM.

LLMs can be accessed via API or we can run one locally.

[Code below](#rag-query-code)

### Step 9 (Query): Answer from LLM

Now we get to see the answer provided by LLM 👏

[Code below](#rag-query-code)

## Code Matrix - OLD

| Example | Documents          | Framework   | Vector DB | Embedding Model             | LLM                 | Code                                                    | Notes                |
|---------|--------------------|-------------|-----------|-----------------------------|---------------------|---------------------------------------------------------|----------------------|
| 1       | Granite Docs (pdf) | None        | Milvus    | BAAI/bge-small-en-v1.5 (OS) | Llama 3 @ Replicate | [code](rag_1_C_dataprepkit_query_llama_replicate.ipynb) | Need REPLICATE TOKEN |
| 2       | Granite Docs (pdf) | llama-index | Milvus    | BAAI/bge-small-en-v1.5 (OS) | Llama 3 @ Replicate | [code](rag_2_llamaindex_milvus_llama_replicate.ipynb)   | Need REPLICATE TOKEN |
| 3       | Milvus FAQ (md)    | None        | Milvus    | BAAI/bge-small-en-v1.5 (OS) | Llama 3 @ Replicate | [code](rag_3_milvus_llama_replicate.ipynb)              | Need REPLICATE TOKEN |
|         |                    |             |           |                             |                     |                                                         |                      |