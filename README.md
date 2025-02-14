# Build a RAG-Powered LLM Service with Ollama & Open WebUI 🚀

## Introduction
Large Language Models (LLMs) are powerful, but they rely solely on their pre-trained knowledge and struggle with retrieving up-to-date or domain-specific information. **Retrieval-Augmented Generation (RAG)** solves this problem by integrating an external retrieval mechanism, allowing LLMs to fetch relevant documents before generating responses.

## How RAG Works
A **RAG pipeline** enhances LLMs by integrating a retrieval step before text generation:
1. User input is converted into an **embedding** using an embedding model.
2. This embedding is compared to a **database of precomputed document embeddings** using similarity search (e.g., cosine similarity).
3. The **most relevant documents** are retrieved and passed to the LLM.
4. The **LLM generates a response**, incorporating the retrieved context.

This approach significantly improves accuracy and reduces hallucinations by grounding the model’s output in factual information.

## What Are We Going to Do?
In this guide, we'll cover:
- Setting up **Ollama** to serve an LLM efficiently
- Configuring **Open WebUI** to interact with the model
- Integrating a **Retrieval-Augmented Generation (RAG)** pipeline to enhance responses
- Optimizing parameters for better accuracy

By the end, you’ll have a working **RAG-powered LLM** integrated with **Open WebUI** for smarter and more context-aware AI responses.

## Ollama and Its Benefits
**Ollama** is a lightweight framework designed for running **LLMs** on local or server environments. It provides an optimized runtime for models like **LLaMA, Mistral, and Gemma**, making it ideal for low-latency inference. 

### Key Benefits of Ollama for RAG Systems:
- **Optimized Model Execution** – Efficient GPU and CPU inference with quantization.
- **Custom Model Management** – Fine-tune and package domain-specific models.
- **Local & Private Deployment** – Ensures data privacy and low-latency processing.
- **Simple API Integration** – Clean REST API for seamless retrieval-based workflows.
- **Supports Embedding Models** – Essential for vector search in RAG applications.

## Open WebUI and Its Role in RAG
**Open WebUI** is an open-source interface for managing and interacting with local or remote LLMs, including those running on **Ollama**.

### Key Benefits:
- **Intuitive Interface** – Browser-based UI for AI interactions.
- **Supports Local & Remote Models** – Compatible with **Ollama, LM Studio**, and API-based models.
- **Easy Model Management** – Switch between LLMs & embedding models easily.
- **Custom Prompting & Plugins** – Better control over LLM behavior.
- **Integration with RAG Pipelines** – Acts as a front-end for retrieval-augmented models.

## Installing Ollama Using Docker
To run Ollama in a containerized environment:

### Step 1: Install Docker
For Debian/Ubuntu:
```bash
sudo apt update && sudo apt install -y docker.io
```
For macOS (via Homebrew):
```bash
brew install docker
```
For Windows: Download and install Docker Desktop from Docker’s official website.

### Step 2: Pull the Ollama Docker Image
```bash
docker pull ollama/ollama:latest
```

### Step 3: Run Ollama Container
```bash
docker run -d --name ollama \
  --gpus=all \
  -p 11434:11434 \
  -v ollama_data:/root/.ollama \
  ollama/ollama:latest
```

### Step 4: Verify Installation
```bash
docker ps
curl http://localhost:11434/api/tags
```
If it returns `[]`, the setup is successful.

## Installing Open WebUI Using Docker
If **Ollama is on your computer**, use:
```bash
docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:main
```

If **Ollama is on a different server**, use:
```bash
docker run -d -p 3000:8080 -e OLLAMA_BASE_URL=https://example.com \
  -v open-webui:/app/backend/data --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:main
```

To run **Open WebUI with Nvidia GPU support**:
```bash
docker run -d -p 3000:8080 --gpus all --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data --name open-webui --restart always \
  ghcr.io/open-webui/open-webui:cuda
```
Now, visit **http://localhost:3000** and set up Open WebUI.

## Pulling a Model in Ollama
To download and run a model, use:
```bash
docker exec -it ollama ollama run llama3.2:3b-instruct-fp16
```

## Integrating RAG via Open WebUI
1. **Create a Knowledge Base** – Upload documents.
2. **Define a Model** – Select a base model and link it to your knowledge.
3. **Test RAG Interactions** – Ask queries and get document-grounded responses.

## Optimizing RAG Parameters
- **Embedding Model** – Change it under **Settings > Admin Settings > Documents**.
- **Top-K** – Set the number of documents retrieved.
- **Chunk Size & Overlap** – Optimize document segmentation for search.
- **LLM Parameters** – Adjust **temperature, max tokens, and response randomness**.

## Using Open WebUI as a REST API Web Service
You can integrate the RAG-powered LLM with other applications via REST API:
```bash
curl http://localhost:11434/api/generate -d '{
  "model": "llama3.2:1b-instruct-fp16",
  "prompt": "Why is the sky blue?",
  "stream": false
}'
```

## Conclusion
Congratulations! 🎉 You've successfully built a **RAG-powered LLM service** with **Ollama & Open WebUI**. This setup enables intelligent, context-aware responses by integrating retrieval mechanisms. Explore **fine-tuning models, optimizing parameters, and scaling** for better performance. 🚀

## References
- [Open WebUI GitHub](https://github.com/open-webui/open-webui)
- [Ollama Docs](https://github.com/ollama/ollama)
