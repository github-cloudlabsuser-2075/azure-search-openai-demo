# Backend Application Documentation

The backend application of this project is built using [Quart](https://quart.palletsprojects.com/), a Python framework for asynchronous web applications. The backend code is stored in the `app/backend` folder.

## Key Components

### ADLSGen2ListFileStrategy
This is a strategy for listing files in Azure Data Lake Storage Gen2. It is defined in `app/backend/prepdocslib/listfilestrategy.py`.

### DocumentAction
This is a class that represents an action to be performed on a document. It is defined in `app/backend/prepdocslib/strategy.py`.

### System Message Chat Conversation
This is a function that is used in multiple places in the backend, specifically in the following files:
- `app/backend/approaches/chatapproach.py`
- `app/backend/approaches/chatreadretrieveread.py`
- `app/backend/approaches/chatreadretrievereadvision.py`

## Customization

The primary backend code you'll want to customize is the `app/backend/approaches` folder, which contains the classes powering the Chat and Ask tabs. Each class uses a different RAG (Retrieval Augmented Generation) approach, which include system messages that should be changed to match your data.

### Chat Approach

The chat tab uses the approach programmed in `chatreadretrieveread.py`. It involves the following steps:

1. It calls the OpenAI ChatCompletion API (with a temperature of 0) to turn the user question into a good search query.
2. It queries Azure AI Search for search results for that query (optionally using the vector embeddings for that query).
3. It then combines the search results and original user question, and calls the OpenAI ChatCompletion API (with a temperature of 0.7) to answer the question based on the sources. It includes the last 4K of message history as well (or however many tokens are allowed by the deployed model).

The `system_message_chat_conversation` variable is currently tailored to the sample data since it starts with "Assistant helps the company employees with their healthcare plan questions, and questions about the employee handbook." Change that to match your data.

## Deployment

The backend is deployed using Azure's Bicep language, as defined in `infra/main.bicep`. The backend is hosted on an Azure App Service, with Python 3.11 as the runtime. The App Service is configured to do build during deployment, and it uses a managed identity for secure access to other Azure resources. The App Service is also configured to be part of a virtual network for enhanced security.