# Profit & Loss Report Chatbot (RAG + SQL Agent)

## Overview

This Jupyter notebook implements a question-answering system for Profit and Loss (P&L) report data analysis. It leverages Large Language Models (LLMs), Google BigQuery Vector Storage, and LangChain to provide intelligent, context-aware responses to financial queries.

## Key Technologies

- **Large Language Models (LLMs)**: Powers natural language understanding and generation.
- **Google BigQuery Vector Storage**: Stores and retrieves pre-analyzed financial data as vectors.
- **LangChain**: Orchestrates the interaction between LLMs, vector storage, and SQL databases.
- **SQL Agent**: Dynamically generates and executes SQL queries based on natural language questions.

## Core Functionality

### 1. Vector Store Retrieval

- Utilizes `BigQueryVectorStore` from LangChain for efficient similarity search.
- Pre-analyzed financial reports are stored as vector embeddings.
- `VertexAIEmbeddings` is used to generate embeddings for queries and documents.

### 2. LLM-Powered Decision Making

- `should_query_vector_store()` function uses an LLM to decide whether to query the vector store or use SQL.
- LLM considers the question content, time frame, and available data to make this decision.

### 3. Dynamic SQL Query Generation

- Employs LangChain's SQL Agent to convert natural language questions into SQL queries.
- `create_sql_agent()` sets up an agent with access to database schema and query execution capabilities.
- `SQLDatabaseToolkit` provides the agent with necessary tools for SQL operations.

### 4. Intelligent Question Processing

- `extract_date_from_question()` uses regex and LLM capabilities to understand temporal aspects of queries.
- `find_matching_values()` identifies relevant financial categories and groups mentioned in the question.

### 5. Answer Generation and Evaluation

- Combines information from vector store and SQL queries to generate comprehensive answers.
- `evaluate_answer_relevance()` uses an LLM to assess the relevance and quality of the generated answer.

## LangChain Components Used

- `ChatOpenAI`: Interface for the LLM (e.g., GPT-4).
- `ConversationChain`: Manages conversation context.
- `LLMChain`: Executes specific LLM tasks like decision making and evaluation.
- `PromptTemplate`: Structures prompts for consistent LLM interactions.
- `SQLDatabase`: Provides an interface to the SQL database.
- `BigQueryVectorStore`: Manages vector storage and retrieval in BigQuery.

## Setup and Configuration

1. Install required packages:
   ```
   pip install langchain langchain-google-vertexai langchain-google-community google-cloud-bigquery sqlalchemy
   ```

2. Set up Google Cloud credentials and BigQuery access.

3. Configure the notebook variables:
   - `project`: Your Google Cloud project ID
   - `dataset`: BigQuery dataset name
   - `service_account_file`: Path to your Google Cloud service account key

4. Set up your OpenAI API key as an environment variable.

## Usage

1. Initialize the notebook components:
   ```python
   main(reload_vector_storage=True)
   ```

2. Start the interactive query session:
   ```python
   question = "What was our revenue in May 2024?"
   response = ask_question(question)
   print(response)
   ```

## Customization

- Modify `vector_store_content_description` to match your financial data structure.
- Adjust `extract_date_from_question()` for different date formats.
- Customize SQL views in `determine_view()` to match your database schema.

## Advanced Features

- **Hybrid Retrieval**: Combines vector similarity search with SQL queries for comprehensive answers.
- **Dynamic Time Awareness**: Automatically adjusts queries based on the time frame mentioned in the question.
- **Relevance Scoring**: Uses LLM to evaluate the quality and relevance of generated answers.

## Contributing

Contributions are welcome! Areas for potential improvement include:
- Enhancing the SQL Agent's query generation capabilities
- Improving the vector store's retrieval accuracy
- Expanding the system to handle more complex financial analyses
