
# AI-Powered GitHub Repository Platform

An AI-powered SaaS application that simplifies GitHub repository exploration through intelligent commit summaries and a context-aware code chatbot. The platform helps developers understand code changes, navigate unfamiliar codebases, and locate specific functionality without manually searching through multiple files.

## Overview

Understanding a large GitHub repository can be time-consuming, especially when developers need to review commit histories, understand existing implementations, or locate specific functionality across multiple files.

To address this challenge, I developed an AI-powered GitHub repository platform using Next.js, tRPC, Prisma, Gemini AI, Octokit, and LangChain.

Users can authenticate through Clerk, create projects by providing a project name and GitHub repository URL, and access existing projects from a dashboard. Each project provides AI-generated summaries of recent commits and a chatbot that answers questions using relevant repository context.

## Key Features

- **User Authentication:** Integrated Clerk to authenticate users and provide access to the application.
- **Project Management:** Allows users to create projects by entering a project name and GitHub repository URL, as well as view and reopen existing projects.
- **AI-Powered Commit Summaries:** Retrieves recent GitHub commits and their diffs, generates summaries using Gemini AI, and stores the results in the database.
- **Structured Commit History:** Displays the 10 most recent commit summaries alongside relevant commit details, such as author and date.
- **Repository-Aware Chatbot:** Answers questions about repository structure, file locations, and code implementations.
- **Semantic File Retrieval:** Uses vector embeddings and cosine similarity to identify the 10 most relevant files for a user's question.
- **Context-Aware Responses:** Supplies retrieved repository content to Gemini AI to generate answers with relevant file names and code context.

## Technology Stack

- **Next.js:** Builds the application interface and application framework.
- **Clerk:** Handles user authentication.
- **tRPC:** Connects frontend operations with backend procedures using type-safe communication.
- **Prisma:** Manages database access and data models.
- **Octokit:** Retrieves GitHub repository information, recent commits, and commit diffs.
- **Gemini AI:** Generates commit summaries, repository file summaries, and chatbot responses.
- **LangChain GitHub Repository Loader:** Loads source files from connected GitHub repositories.
- **Google `text-embedding-004`:** Converts file summaries and user questions into vector embeddings.
- **Cosine Similarity:** Compares question embeddings with stored file embeddings to retrieve relevant repository files.

## Architecture Diagram

<img width="1774" height="887" alt="image" src="https://github.com/user-attachments/assets/6fe7865a-9929-4ec6-a281-a3c89454a1fb" />

## How It Works

### 1. Authentication and Project Management

- Users sign in through Clerk authentication.
- After authentication, users access the project dashboard.
- The dashboard displays existing projects and provides an option to create a new project.
- To create a project, users enter a project name and the URL of the GitHub repository.
- Project details are stored in the database and associated with the application workflow.
- Users can open an existing project to access its commit summaries and code chatbot.

### 2. AI-Powered Commit Summarization

The commit summarization workflow helps developers understand repository changes without manually reviewing every commit diff.

- **Retrieve repository data:** The backend uses Octokit to access the connected GitHub repository and retrieve recent commits.
- **Extract code changes:** Commit diffs provide information about changes introduced in each commit.
- **Generate summaries:** The commit information and diffs are sent to Gemini AI to generate concise summaries.
- **Store results:** Generated commit summaries are stored in the database.
- **Display summaries:** The application retrieves the stored summaries and presents the 10 most recent commits in a structured interface.

This workflow makes it easier to review recent development activity and understand what changed in a repository.

### 3. Repository Indexing and File Summarization

The chatbot needs repository context to answer questions about source code. To support this, the application creates searchable representations of repository files.

- **Load repository files:** LangChain's GitHub repository loader retrieves files from the connected repository.
- **Summarize file contents:** Gemini AI generates concise summaries of the files, making their purpose and functionality easier to retrieve.
- **Generate embeddings:** Google's `text-embedding-004` model converts the file summaries into vector embeddings.
- **Store searchable information:** The embeddings and associated file metadata are stored in the database.

These stored representations allow the application to retrieve potentially relevant files without relying solely on direct searches for matching words.

### 4. AI-Powered Code Chatbot

The chatbot uses Retrieval-Augmented Generation (RAG) to answer questions using relevant information from the connected repository.

- **Accept a question:** The user asks a question, such as "Where is the authentication logic implemented?" or "Which file handles API requests?"
- **Generate a question embedding:** The question is converted into a vector using the embedding model.
- **Search for relevant files:** Cosine similarity compares the question vector with the stored repository file embeddings.
- **Retrieve context:** The top 10 most relevant files are selected based on similarity.
- **Generate an answer:** The selected repository context and the user's question are provided to Gemini AI.
- **Return file references:** The chatbot generates an answer that helps the user identify relevant file names and understand the corresponding implementation.

This approach helps developers navigate unfamiliar repositories and understand where particular functionality is implemented.

## Data Flow

### Project Creation and Repository Indexing

1. User enters the project name and GitHub URL.
2. The backend processes the project creation request.
3. Project information is stored in the database.
4. LangChain loads the repository files.
5. Gemini AI summarizes the file contents.
6. `text-embedding-004` generates vector embeddings.
7. Embeddings and file metadata are stored for retrieval.

### Commit Summarization

1. Octokit retrieves recent commits and their diffs.
2. Gemini AI generates concise summaries of the changes.
3. The summaries are saved in the database.
4. The application retrieves the stored summaries.
5. The 10 most recent commit summaries are displayed in the project interface.

### Chatbot Question and Answer

1. The user submits a code-related question.
2. The question is converted into a vector embedding.
3. Cosine similarity identifies relevant stored file embeddings.
4. The top 10 relevant files are retrieved.
5. Gemini AI receives the question and selected repository context.
6. The chatbot returns an answer with relevant file names and code context.

## Benefits

- **Faster Repository Exploration:** Reduces the effort required to understand recent changes and navigate codebases.
- **Improved Code Understanding:** Provides concise commit summaries and repository-aware answers.
- **Relevant File Discovery:** Helps developers identify files associated with specific features or implementations.
- **Context-Aware AI Assistance:** Uses retrieved repository information to ground chatbot responses.
- **Improved Collaboration:** Helps frontend, backend, and other developers understand shared repositories more efficiently.

## Setup and Configuration

### Prerequisites

- Node.js and the package manager used by the project.
- A configured database.
- Clerk application credentials.
- Gemini AI API credentials.
- GitHub access configured for repository retrieval.

### Installation

1. Clone the repository.
2. Install dependencies using the package manager configured in the project.
3. Configure the required environment variables.
4. Run the development command defined in `package.json`.

Use the actual scripts and environment variable names from the project configuration when setting up the application.

### Environment Variables

The application may require configuration for:

- Clerk authentication
- Database connection
- Gemini AI API access
- GitHub repository access, where required

Keep credentials and API keys in environment variables. Do not commit secrets to the repository.

## Future Enhancements

- Add incremental repository indexing when new commits are pushed.
- Improve retrieval accuracy for large repositories.
- Support filtering commit summaries by author, date, or branch.
- Add conversation history for chatbot sessions.
- Improve source-code references and navigation from chatbot answers.
- Add background processing for repository indexing and summary generation.

## Conclusion

The AI-Powered GitHub Repository Platform combines GitHub integration, large language models, embeddings, and semantic retrieval to simplify repository exploration.

By storing AI-generated commit summaries and using RAG to retrieve relevant source files, the application helps developers understand code changes, locate implementations, and navigate shared codebases more efficiently.
