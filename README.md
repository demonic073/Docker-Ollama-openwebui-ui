# docker compose ollama and open-webui ui

I built this docker-compose deployment to isolate any outside connections within the docker container to make sure all queries ran on the ai model are only ran local nothing is getting sent outside. I mostly used this to test Deepseek ai model, but this can also be used with any support Olama compatible models as well.  

Nivida GPUS as that is what I currently run you will need to look up how to set other GPUS yourself. 

# Prerequisites
* Ubuntu 24.04.1 Native or WSL 
* Prerequisites
* [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installation)
* [Docker Compose](https://docs.docker.com/compose/install/)

# Deployment

1. clone this repo

2. start the docker cotainers:

   ```sh
    docker-compose exec ollama ollama list
   

3. Next, pull the model you want to test (in this case, I am using `deepseek-r1:8b`):

   ```sh
   docker-compose exec ollama ollama pull deepseek-r1:8b

4. check the model is loaded in Ollama:
   
   ```sh
   docker-compose exec ollama ollama list

You should see:

<pre>
NAME              ID              SIZE      MODIFIED
deepseek-r1:8b    28f8fd6cdc67    4.9 GB    2 minutes ago
</pre>

5. log into the Open-webui in your local browser
   http://localhost:8080/