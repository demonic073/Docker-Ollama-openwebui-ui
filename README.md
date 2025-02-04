# Docker Compose Ollama and Open-webui UI

I built this docker-compose deployment to isolate any outside connections within the docker container to make sure all queries ran on the ai model are only ran local and nothing is getting sent outside. I mostly used this to test Deepseek ai model, but this can also be used with any supported Ollama compatible models as well.  

This is for Nivida GPUS as that is what I currently run I also created Intel GPU deployment that worked on older Intel Mac book pro not sure if it would work on any of the M Mac book chipsets. 

# Prerequisites
* Ubuntu 24.04.1 Native or WSL [How to install WSL](https://learn.microsoft.com/en-us/windows/wsl/install)
* [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installation)
* [Docker Compose](https://docs.docker.com/compose/install/)

# Deployment

1. clone this repo and cd Docker-Ollama-openwebui-ui/

2. start the docker cotainers:

   ```sh
    docker-compose up -d

<b>For INTEL GPU version</b>
```docker-compose -f docker-compose-intel.yml up -d```

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

# Monitoring

You can montitor the connections to make sure the containers are not talking to the internet and only locally on your machine

1. Show Only Source & Destination IPs

    ```sh
    sudo tcpdump -i any port 11434 -n -q

2. Show Only Unique IP Addresses

    ```sh
    sudo tcpdump -i any port 11434 -n | awk '{print $3, $5}' | cut -d'.' -f1-4 | sort -u

3. Show Only Outbound Destination IPs

    ```sh
    sudo tcpdump -i any port 11434 -n | awk '{print $5}' | cut -d'.' -f1-4 | sort -u

# Tweaks 

To get more CPU power and quick response there are 2 options you can tweak in Ollama in the docker-compose.yml file, I had added base config of 8GB of memory and 4 CPU which is quite slow but is safe load under lower powered machines. 

Memory based on the amount of memory you have for your machine example memory:
Example: ```memory: 16g```

also you can tweak the CPU setting based off how many cores your CPU supports 
Example: ```cpus: '12'```

# TIPS 

I had some issues trying to get the NVIDIA Drivers working with WSL

.1 Add this repo

```
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | \
    sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg && \
    curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

.2 next run the following command
```
sudo nvidia-ctk runtime configure --runtime=docker
```

.3 restart docker service 
```
sudo systemctl restart docker
```


   
