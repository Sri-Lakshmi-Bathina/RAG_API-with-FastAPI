<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a RAG API with FastAPI

**Project Link:** [View Project](http://learn.nextwork.org/projects/ai-devops-api)

**Author:** Sri Lakshmi Bathina  
**Email:** sbathina02@gmail.com

---

![Image](http://learn.nextwork.org/delighted_maroon_festive_grape/uploads/ai-devops-api_g3h4i5j6)

---

## Introducing Today's Project!

In this project, I will demonstrate how to build a RAG API from scratch. I'm doing this project to learn how RAG works, how to use Ollama, and how to build an API and run it locally.

### Key services and concepts

Services I used were Ollama, FastAPI, uvicorn, Swagger UI, and Chroma to build our RAG API. Key concepts I learnt include how RAG works, how APIs work, different types of endpoints and methods, and how to test our own endpoints.

### Challenges and wins

This project took me approximately 2.5 hours, including demo and secret mission time. The most challenging part was understanding how the FastAPI framework works - learning how it abstracts work away from engineers around routing HTTP requests, generating documentation, etc. was a huge unlock. It was most rewarding to test our extra endpoint(/add) to add new information into the knowledge base dynamically.

### Why I did this project

I did this project because I wanted to learn how APIs work and build an AI API from scratch. Looking forward to trying the project on containerizing my API using Docker.

---

## Setting Up Python and Ollama

In this step, I'm setting up Python and Ollama. Python is the programming language we're using to build the RAG API. Ollama is the tool that helps us with installing and accessing LLMs locally. We need these tools because our RAG API needs both equally - we need Python for the API code to work, and we need  LLMs to transform the raw data in the knowledge base into personalized responses.

### Python and Ollama setup

![Image](http://learn.nextwork.org/delighted_maroon_festive_grape/uploads/ai-devops-api_i9j0k1l2)

### Verifying Python is working

### Ollama and tinyllama ready

Ollama is a tool for installing and running LLMs. I downloaded the tinyllama model because it is lightweight(~600MB compared to 2GB for other models), but it still works well for this project. The model will help the RAG API by converting raw chunks of data in the knowledge base into cohesive, insightful answers.

---

## Setting Up a Python Workspace

In this step, I'm setting up a Python workspace, which is a virtual environment. A virtual environment is an isolated workspace for each of our coding projects. We need it because it prevents package version conflicts between different projects. It's also handy that at the end of the project, if we no longer need it, deleting the environment deletes all the dependencies automatically, which is more convenient than deleting each dependency individually.

### Python workspace setup

### Virtual environment

A virtual environment is an isolated workspace for Python projects that stores the packages/dependencies we need for this project to run. 
I created one for this project because it prevents any package we install from creating any error in other Python projects we might have on our computer(this also makes it easier to remove the dependencies used at the end of the project completion).
Once I activate it, the terminal uses the Python and Packages from this specific environment, ensuring the project dependencies stay isolated and the computer stays tidy. 
To create a virtual environment, I used Python to create a folder (usually called venv) that contains its own copy of Python and a place to install packages. When we "activate" the virtual environment, the terminal uses the Python and packages from that folder instead of the system-wide Python.

### Dependencies

The packages I installed are FastAPI, Chromadb, Uvicorn, and Ollama. FastAPI is a web framework used for developing APIs. 
Chroma is used for storing and processing the embeddings in the knowledge base. 
Uvicorn is used to run our FastAPI app and makes it accessible locally on our computer. 
Ollama is used to give the Python API a way to send questions to tinyllama and get responses programmatically.

![Image](http://learn.nextwork.org/delighted_maroon_festive_grape/uploads/ai-devops-api_u1v2w3x4)

---

## Setting Up a Knowledge Base

In this step, I'm creating the knowledge base. A knowledge base is a collection of information that is provided to an AI model to give it accurate and up-to-date context on specific topics. I need it because AI models like tinyllama have limited knowledge from their training data. By providing our own knowledge base, the model has accurate and up-to-date information.

### Knowledge base setup

![Image](http://learn.nextwork.org/delighted_maroon_festive_grape/uploads/ai-devops-api_t1u2v3w4)

### Embeddings created

Embeddings are numerical representations of text that helps the RAG API quickly find relevant information when answering questions. I created them by running the embed.py script, which reads the knowledge base (k8s.txt) and stores these numerical representations in ChromaDB for semantic search. The db/ folder contains chroma.sqlite3 and a subfolder with embeddings This is important for RAG because they allow the system to understand the meaning behind our questions and the content in our knowledge base. 

---

## Building the RAG API

In this step, I'm building an RAG API. An API (Application Programming Interface) is a set of rules and definitions that allows different software applications to communicate with each other.  FastAPI is a modern, fast (high-performance) web framework for building APIs with Python. I'm creating this because it will serve as the interface for the RAG system. It will combine retrieval (searching the knowledge base) with generation (creating answers using AI). 

### FastAPI setup

### How the RAG API works

My RAG API works by sending a question to our server and then breaking it down into embeddings using Chroma. Chroma will then compare the embeddings of our question with those of the documents stored in our knowledge base. The results for relevant information get passed as context to our tinyllama LLM, alongside the original question, and the prompt to "Answer clearly and consistently." The endpoint is /query, and we built it using a POST HTTP method because we are sending data(i.e., the question) to this API.

![Image](http://learn.nextwork.org/delighted_maroon_festive_grape/uploads/ai-devops-api_f3g4h5i6)

---

## Testing the RAG API

In this step, I'm testing my RAG API. I'll test it using Swagger UI. Swagger UI is an interactive, browser-based documentation for the API. I'll use it to visualize and interact with the API's endpoint without writing any code.

### Testing the API

### API query breakdown

I queried my API by running the command: curl -X POST "http://127.0.0.1:8000/query" -G --data-urlencode "q=What is Kubernetes?". The command uses the POST method, which means we are sending the question "What is Kubernetes?" as data to the /query endpoint of the RAG API. The API responded with a JSON object containing the AI-generated answer based on the knowledge base.

![Image](http://learn.nextwork.org/delighted_maroon_festive_grape/uploads/ai-devops-api_g3h4i5j6)

### Swagger UI exploration

Swagger UI is an interactive, browser-based documentation for our API. I used it to test the RAG API by visiting localhost:8000/docs in my browser, expanding the POST /query endpoint, entering a question like What is Kubernetes? in the q parameter field, and then clicking Execute. The best part about using Swagger UI was that it provided a much more visual and interactive way to test our API compared to using curl commands. It makes it easy to see all available endpoints, HTTP methods, required parameters, and response formats, and allows you to test them directly in the browser.

---

## Adding Dynamic Content

In this project extension, I am updating the RAG API by creating a new endpoint that makes it easy to update our knowledge base. /add will let us add new information through an API endpoint instead of uploading a new text file into our project's /db folder. It also saves us the effort of re-running the embeddings script.

### Adding the /add endpoint

![Image](http://learn.nextwork.org/delighted_maroon_festive_grape/uploads/ai-devops-api_w9x0y1z2)

### Dynamic content endpoint working

The /add endpoint allows me to dynamically add new content to your knowledge base directly through the RAG API. This is useful because it means we no longer have to manually edit the files or re-run scripts to update our knowledge base. Instead, other applications can use this endpoint to expand our knowledge base in real-time, making the RAG system more flexible and production-ready.

---

---
