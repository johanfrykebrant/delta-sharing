# About

Delta Sharing is an open source protocol that allows for secure sharing of large data sets that are stored on the cloud.
In order to further understand what delta sharing is and how it functions, this example will provide instructions on how to set up a Delta Sharing Server on your local machine.
This is just for testing purposes and nothing else. For this to be of any practical use, the Delta Sharing Server must be accessible via the internet. See link 4 on how to host your the Delta Sharing Server on an Azure VM.

# Requirements

- An Azure Deta Lake Storage V2 with a container that holds one or more folders with data stored in delta table format. 


# Installation & set-up uf docker container

- Install docker desktop (Requires WSL2(Windows Subsystem for Linux) to start up properly, but will be disabled later)

- Enable Hyper-v by using this command in power shell (run as admin):
```bash
    Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```
- Disable WSL 2 in docker desktop. Click on Settings and unselect 'Use the WSL 2 based engine'

- Download the config and profiles folder to your machine.

- Fill in missing information in `core-site.xml`, `delta-sharing-server-config.yaml` and `profile.share`. Below are the missing variables for each file.
    - `core-site.xml`
        - `<storage account name>`
        - `<shared key string>`
    - `delta-sharing-server-config.yaml` (See link 4 for other storage alternatives)
        - `<share name>`
        - `<schema name>`
        - `<table name>`
        - `<container name>`
        - `<storage account name>`
        - `<table path>`
        - `<docker host IP>` should be localhost if running on your machine
        - `<token>`
    - `profile.share`
        - `<docker host IP>` same as above
        - `<token>`

- Allow docker containers to access the directory containing `core-site.xml` and `delta-sharing-server-config.yaml`. Settings > Resources > File sharing > Click on the plus sign enter the path to the directory.

- Retrive the docker image by running the following command in powershell. Note that you have to replace `<path>` with the path to the config folder.
```bash
    docker run --name deltasharingcontainer \ 
    -p 8080:8080 \
    --mount type=bind,source=<path>\config\delta-sharing-server-config.yaml,target=/config/delta-sharing-server-config.yaml  \
    --mount type=bind,source=<path>\config\core-site.xml,target=/opt/docker/conf/core-site.xml deltaio/delta-sharing-server:0.3.0 \
    -- --config /config/delta-sharing-server-config.yaml
```
- Docker container is now up and running on your machine. See Delta sharing.ipynb on how to retrive data from cloud storage using delta sharing.

## Useful links:
1. https://blog.devgenius.io/delta-sharing-on-azure-a64a4fb8b0c2 (How to host your docker container on an Azure VM)
2. https://github.com/delta-io/delta-sharing/blob/main/README.md
3. https://hadoop.apache.org/docs/stable/hadoop-azure/abfs.html#Authentication (Authentication alternatives)
4. https://github.com/delta-io/delta-sharing/blob/main/server/src/universal/conf/delta-sharing-server.yaml.template (Different cloud storage alternatives)
