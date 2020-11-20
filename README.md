# Ask Me Anything Chatbot

## Introduction
The Chatbot has been developed for [Kata.ai](https://kata.ai/platform) chatbot framework.
It is a chatbot with the "Open Domain Long Form Question and Answering" (ODLFQA) function, in which 
a user (e.g. a child or a student) can ask anything and the chatbot engine can give a long answer. 

The chatbot's ODLFQA engine gathers relevant information from a variety of sources (e.g. paragraphs from 
relevant Wikipedia pages), synthesizes the information, and creates an easy-to-read original 
summary of the relevant elements. The engine uses various advanced techniques such as pre-trained 
language models (e.g. BART, T5) for conditional text generation and a new method for finding 
information in large data sources (such as Wikipedia) using the REALM or DPR model.

Big thanks to [Yacine Jernite](https://yjernite.github.io/) from Huggingface who provides the code for "Open Domain Long Form Question and Answering"
as [jupyter notebook](https://yjernite.github.io/lfqa.html).

## Chatbot's Usage
Currently the Chatbot is available on Telegram messenger as [@UncoolAIBot](https://t.me/UncoolAIBot) 
or you can join the Telegram group [AMAPlanet](https://t.me/AMAPlanet). Following chatbot's commands are
available now:
- /start

  It is the welcome action when you join to the chatbot
- /about

  Explain about the chatbot
- /ask <question>

  Ask a question, example: /ask how old is the universe?
  If you talk with the chatbot using private message, you can ask a question without the /ask command
- /joke

  Just give a random joke available on internet for entertainment.
  
## Architecture
### Chatbot's Framework
It is running on kata.ai server infrastructure which works as the frontend and "hub" between the chat messenger 
(Telegram in this case) and the Chatbot's ODLFQA Engine
### Chatbot's ODLFQA Engine
The engine has been dockerized and available at https://hub.docker.com/repository/docker/wirawan/eli5.

#### Docker Usage
- Create a directory /eli5/.cache (or another directory where you have access) before you run the docker for the first time,
It needs around 25GB disk space:
 
  `% mkdir -p /eli5/.cache`
 
- run the docker image **wirawan/eli5**:
 
  `% docker run --gpus all -p 8000:8000 -v /eli5/.cache:/root/.cache wirawan/eli5`. 

It will run the engine on port 8000 and provide Rest API interface on `http://<hostname>:8000/ask/` and its documentation: 
`http://<hostname>:8000/docs/`. 

We will use this URL address `http://<hostname>:8000/ask/` in chatbot 
config file bot.yml in the action `get_eli5`.
The first time we run `docker run --gpus all -p 8000:8000 -v /eli5/.cache:/root/.cache wirawan/eli5`, 
it will download all necessary models and datasets, and prepare it for the inferencing. This will take more 
than 2 hours depend on your network and server performance. This long process will run only once. All 
processed files are saved in local host file system /eli5/.cache, so we can use it for the following run without 
long pre-processing time. 

#### Server requirement
- a Windows/Linux server with docker, 100GB Disk space and 16GB RAM
- NVidia GPU (16GB RAM)



