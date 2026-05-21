# Cursor Knowledge Base
How to structure your Claude folder effectively - [Anatomy of Claude folder](https://blog.dailydoseofds.com/p/anatomy-of-the-claude-folder)

Get Publicly available skills from [skill dot sh] (skills.sh)

# AI setup for python based Agentic Workflow Development
my go to guide to setting up a python project and include a barebone openai execution

## Activating a Virtual Environment in Windows

```python -m venv .venv```
```.venv\Scripts\activate```

## Install dependencies
```pip install openai```

## Write to requirements.txt file
```pip freeze > requirements.txt```

## Run Docker ollama image with GPUs
```docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama```

If ollama image is not found, explicity download the image
## Check if image is available
```docker exec -it ollama ollama list```
## Explicitly download the image
```docker exec -it ollama ollama pull gemma4:latest```

## Run Claude CLI with local model running on docker #
Have qwen3.6 model downloaded and up and running on ollama
Install Claude Code via terminal:
Mac/Linux/WSL: ```curl -fsSL https://claude.ai/install.sh | bash.```

Windows (PowerShell): ```irm https://claude.ai/install.ps1 | iex```

execute claude in your desired directory
```ollama launch claude --model qwen3.6:latest```

## Connect Ollama local LLM model with Claude CLI
The process described below is to connect the local ollama model with claude cli, this approach will require you to set ollama as a docker image and it is a pre-requisite

Install Claude CLI
Download latest qwen model in ollama

Run the command to check the model status after ollama is up and running
```ollama ps```

Check the context length, usually it will be around 4000, however it is insufficient for claude cli operations as it requires upwards of 65k context window.
To increase the context length to 65k we need to create a model file (filename 'ModelFile' with no extension) with the following content
```
dockerfile
FROM qwen3.6:latest
PARAMETER num_ctx 65000
```
Run the model

```ollama create qwen3.6-arch -f ModelFile```

```ollama run qwen 3.6-arch```

verify that the model is up and running
Execute claude

```ollama launch claude --model qwen3.6-arch```

