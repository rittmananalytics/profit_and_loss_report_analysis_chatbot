# Profit & Loss Report Self-Learning Chatbot (RAG + SQL Agent)

## Overview

This Jupyter notebook implements an advanced question-answering system for Profit and Loss (P&L) data analysis. It combines Retrieval-Augmented Generation (RAG), vector store queries, and SQL-based data retrieval to provide comprehensive and accurate answers to financial questions.

## Key Features

1. Hybrid RAG and SQL-based querying
2. Dynamic decision-making between RAG and SQL approaches
3. Feedback loop for continuous improvement
4. Vector store for storing and retrieving successful Q&A pairs
5. Flexible time period handling for financial queries

## System Architecture

### 1. Vector Stores

The system uses two vector stores:
- `vector_store`: Stores pre-created P&L report analyses
- `qa_vector_store`: Stores successful question-answer pairs

Both vector stores are implemented using BigQuery and the `BigQueryVectorStore` class from LangChain.

### 2. Embedding Model

We use the `VertexAIEmbeddings` model to generate embeddings for both the pre-created analyses and the Q&A pairs.

### 3. Language Model

The system uses OpenAI's GPT-4 model (via `ChatOpenAI`) for natural language understanding and generation.

### 4. SQL Database

The P&L data is stored in BigQuery tables, accessed through SQLAlchemy.

## Workflow

1. **Question Input**: The user inputs a question about P&L data.

2. **RAG vs SQL Decision**:
   - The `should_query_vector_store` function determines whether to use RAG or SQL-based querying.
   - It considers the question content, time frame, and available data in the vector store.

3. **RAG Approach** (if chosen):
   - The system queries the `vector_store` for relevant pre-created analyses.
   - It uses the `similarity_search` method to find the most relevant document.
   - The retrieved content is summarized and presented as the answer.

4. **SQL Approach** (if chosen or if RAG fails):
   - The system extracts relevant time periods and financial categories from the question.
   - It constructs a SQL query using the `SQLDatabaseToolkit` and `create_sql_agent` from LangChain.
   - The query is executed against the BigQuery database, and the results are processed to form an answer.

5. **Answer Evaluation**:
   - The `evaluate_answer_relevance` function uses the language model to assess the relevance and quality of the generated answer.

6. **User Feedback**:
   - The system asks the user if the answer is sufficient.
   - If not, it requests feedback on how to improve the answer.

7. **Question Refinement**:
   - If the user is not satisfied, the system uses the language model to generate an improved question based on the feedback.
   - The process repeats with the refined question (up to a maximum of 3 iterations).

8. **Storing Successful Q&A Pairs**:
   - When a user is satisfied with an answer, the Q&A pair is stored in the `qa_vector_store`.
   - This stored pair can be used to inform future similar questions.

## Key Components

### 1. `extract_time_periods` Function
Identifies and extracts various time periods (years, months, quarters) mentioned in the question.

### 2. `construct_date_filter` Function
Constructs SQL date filters based on the extracted time periods.

### 3. `ask_question` Function
The core function that orchestrates the question-answering process, including the decision between RAG and SQL approaches.

### 4. `ask_question_with_feedback_and_learning` Function
Implements the feedback loop and question refinement process.

### 5. `store_successful_qa` Function
Stores successful Q&A pairs in the vector store for future reference.

## Continuous Learning

The system improves over time through two mechanisms:
1. Storing successful Q&A pairs in the vector store, which can be retrieved for similar future questions.
2. The feedback loop, which allows for question refinement based on user input.

## Usage

1. Set up the required Google Cloud and OpenAI credentials.
2. Run the notebook cells in order to initialize all components.
3. Use the `main` function to start an interactive Q&A session about P&L data.

## Customization

The system can be customized by:
- Modifying the `vector_store_content_description` to match specific P&L data structures.
- Adjusting the SQL views and table names in the `determine_view` function.
- Expanding the `extract_time_periods` function to handle more complex date formats.
