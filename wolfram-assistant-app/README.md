# Wolfram Assistant Agent

## Overview

This project implements a Wolfram Assistant Agent, leveraging Wolfram Alpha's conversational API to provide answers to user queries. The agent manages the interaction between user input and Wolfram Alpha's responses, enhancing the dialogue with refined language processing to deliver a smooth and engaging user experience.

## Features
* Utilizes Wolfram Alpha's conversational API for accurate and detailed responses.
* Orchestrates the interaction with a language model for enhanced user engagement.
* Built using FastAPI for robust and fast web server performance.
* Includes observability and logging for monitoring and debugging.

## Getting Started

### Prerequisites

* We assume you have already cloned the repository, as explained in the general README. For the sake of clarity, here's the command again:
   ```
   git clone --recursive https://github.com/8090-inc/xrx-sample-apps.git
   ```

* If the submodule was not installed, or you want to update it, use the following command:
   ```
   git submodule update --init --recursive
   ```

* Install `Docker`, `Python3`, and `Pip3` with [homebrew](https://formulae.brew.sh/) on macOS or `apt-get update` on Debian/Ubuntu based Linux systems:

    ```bash
    brew cask install docker
    brew install python@3.10
    ```


* Create your `WOLFRAM_API_ID` from [Wolfram Alpha's API dashboard](https://developer.wolframalpha.com/portal/myapps/) and set the `WOLFRAM_BASE_URL` to `http://api.wolframalpha.com/v1/conversation.jsp` as environment variables. Select the conversational API.
   ```env
   WOLFRAM_API_ID="YOUR_WOLFRAM_API_ID"
   WOLFRAM_BASE_URL="http://api.wolframalpha.com/v1/conversation.jsp"
   ```

## How To Run

### Using Docker

1. Build the Docker image:
   ```bash
   docker build -t wolfram-assistant-agent:latest .
   ```

2. Run the container:
   ```bash
   docker run -p 8003:8003 --env-file .env wolfram-assistant-agent:latest
   ```

The agent will be accessible at `http://localhost:8003`.

### Locally without Docker

1. Set up the Python virtual environment:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

2. Install requirements:
   ```bash
   pip install -r requirements.txt
   ```

3. Start a local redis cluster
   ```bash
   redis-server --port 6379
   ```

4. Run the application:
   ```bash
   cd app
   uvicorn main:app --host 127.0.0.1 --port 8003 --reload
   ```

The agent will now be running at `http://localhost:8003`


## API Usage

The agent exposes a single endpoint using FastAPI:

- `POST /run-reasoning-agent`: Submit a query to the reasoning agent and receive streaming responses.

Example request using `curl`:
```bash
curl -X POST http://localhost:8003/run-reasoning-agent \
     -H "Content-Type: application/json" \
     -H "Accept: text/event-stream" \
     -d '{
           "session": {
               "id": "1234567890"
           },
           "messages": [
               {"role": "user", "content": "Explain the theory of relativity."}
           ]
         }'
```

To view the interactive API documentation, visit `http://localhost:8003/docs` in your web browser after starting the server.

## Environment Configuration

Create a `.env` file in the root directory of your project. Use the provided `env-example.txt` as a template. Here's a minimal example:
```env
# Wolfram Alpha Configuration
WOLFRAM_API_ID="YOUR_WOLFRAM_API_ID"
WOLFRAM_BASE_URL="http://api.wolframalpha.com/v1/conversation.jsp"

# LLM Configuration (if applicable)
LLM_API_KEY="your_llm_api_key_here"
LLM_BASE_URL="https://api.example.com/v1"
LLM_MODEL_ID="model-id"

# Agent Configuration
INITIAL_RESPONSE="Hello! I'm the Wolfram Assistant. How can I help you today?"

# Observability Configuration
LLM_OBSERVABILITY_LIBRARY="none"

```

## Project Structure

- `reasoning/`: Contains the main application code
  - `app/`: Backend application folder
    - `agent/`: Agent logic
      - `executor.py`: Main agent execution logic
      - `tools/`: Folder containing agent tools
        - `generic_tools.py`: Generic tools for the agent
    - `__init__.py`: Initializes the app module
    - `main.py`: FastAPI application setup and endpoint definition
  - `Dockerfile`: Docker configuration for containerization
  - `requirements.txt`: Python dependencies
- `nextjs-client/`: Next.js frontend for the Wolfram assistant application
- `test/`: Contains test files
- `xrx-core/`: Core xRx framework (submodule)
- `docker-compose.yaml`: Docker Compose configuration file
- `env-example.txt`: Example environment variables file
- `README.md`: Project documentation

## Troubleshooting
If you encounter any issues, please check the following:

* Ensure all environment variables are correctly set.
* Verify that all dependencies are installed correctly.
* Check the logs for any error messages.

## Contributing

Contributions to improve the Wolfram Assistant App are welcome. Please follow these steps:

1. Open a new issue on GitHub describing the proposed change or improvement
2. Fork the repository
3. Create a new branch for your feature
4. Commit your changes
5. Push to your branch
6. Create a pull request, referencing the issue you created

> **Note:** pull requests not backed by published issues will not be considered. This process ensures that all contributions are discussed and aligned with the project's goals before implementation.
