# Backend Application

The backend application is built using [Quart](https://quart.palletsprojects.com/), a Python framework for asynchronous web applications. The backend code is stored in the `app/backend` folder. The frontend and backend communicate using the [AI Chat HTTP Protocol](https://aka.ms/chatprotocol).

## Directory Structure

The primary backend code you'll want to customize is the `app/backend/approaches` folder, which contains the classes powering the Chat and Ask tabs. Each class uses a different RAG (Retrieval Augmented Generation) approach, which include system messages that should be changed to match your data.

## Customizing the Backend

### Chat/Ask tabs

The chat tab uses the approach programmed in [`chatreadretrieveread.py`](app/backend/approaches/chatreadretrieveread.py).

1. It calls the OpenAI ChatCompletion API (with a temperature of 0) to turn the user question into a good search query.
2. It queries Azure AI Search for search results for that query (optionally using the vector embeddings for that query).
3. It then combines the search results and original user question, and calls the OpenAI ChatCompletion API (with a temperature of 0.7) to answer the question based on the sources. It includes the last 4K of message history as well (or however many tokens are allowed by the deployed model).

The `system_message_chat_conversation` variable is currently tailored to the sample data since it starts with "Assistant helps the company employees with their healthcare plan questions, and questions about the employee handbook." Change that to match your data.

If you followed the instructions in [`docs/gpt4v.md`](docs/gpt4v.md) to enable a GPT Vision model and then select "Use GPT vision model", then the chat tab will use the `chatreadretrievereadvision.py` approach instead. This approach is similar to the `chatreadretrieveread.py` approach, with a few differences.