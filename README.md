# docker compose olima and open-webui ui

I built this docker-compose deployment to isolate any outside connections within the docker container to make sure all queries ran on the ai model are only ran local nothing is getting sent outside. I mostly used this to test Deepseek ai model, but this can also be used with any support Olama compatible models as well.  

# Prerequisites
Prerequisites
[NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installation)
[Docker Compose](https://docs.docker.com/compose/install/)

