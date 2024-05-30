# Backend Application

This directory contains the backend application for our project, built using [Quart](https://quart.palletsprojects.com/), a Python framework for asynchronous web applications.

## Key Components

### ADLSGen2ListFileStrategy

This strategy, defined in `app/backend/prepdocslib/listfilestrategy.py`, is used for listing files in Azure Data Lake Storage Gen2.

### DocumentAction

This class, defined in `app/backend/prepdocslib/strategy.py`, represents an action to be performed on a document.

### System Message Chat Conversation

This function is used in multiple places in the backend, specifically in the following files:
- `app/backend/approaches/chatapproach.py`
- `app/backend/approaches/chatreadretrieveread.py`
- `app/backend/approaches/chatreadretrievereadvision.py`

## Customization

The primary backend code you'll want to customize is the `app/backend/approaches` folder. This folder contains the classes powering the Chat and Ask tabs. Each class uses a different RAG (Retrieval Augmented Generation) approach, which include system messages that should be changed to match your data.

### Chat Approach

The chat tab uses the approach programmed in `chatreadretrieveread.py`. It involves the following steps:

1. It calls the OpenAI ChatCompletion API (with a temperature of 0) to turn the user question into a good search query.
2. It queries Azure AI Search for search results for that query (optionally using the vector embeddings for that query).
3. It then combines the search results and original user question, and calls the OpenAI ChatCompletion API (with a temperature of 0.7) to answer the question based on the sources. It includes the last 4K of message history as well (or however many tokens are allowed by the deployed model).

## Getting Started

To get started with the backend application, follow these steps:

1. Install the required Python packages by running `pip install -r requirements.txt`.
2. Start the Quart server by running `python main.py`.

For more detailed instructions, refer to the [main README](../README.md) file.