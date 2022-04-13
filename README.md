- Install docker desktop (Requires WSL2(Windows Subsystem for Linux) to start up properly, but will be disabled later)

- Enable Hyper-v by using this command in power shell (run as admin):
    Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All

- Disable WSL 2 in docker desktop. Click on Settings and unselect 'Use the WSL 2 based engine'

- Create a local directory and save  the config folder and its content to that directory.

- Fill in missing information in core-site.xml, delta-sharing-server-config.yaml and profile.share
    - core-site.xml
        - <storage account name>
        - <shared key string>
    - delta-sharing-server-config.yaml (See link 4 for other storage alternatives)
        - <share name>
        - <schema name>
        - <table name>
        - <container name>
        - <storage account name>
        - <table path>
        - <docker host IP>
        - <token>
    - profile.share
        - <docker host IP>
        - <token>

- Allow docker containers to access the directory. Settings > Resources > File sharing > Click on the plus sign enter the path to the directory.

- Retrive the docker image by running the following command in powershell
    docker run --name deltasharingcontainer \ 
    -p 8080:8080 \
    --mount type=bind,source=<path>\config\delta-sharing-server-config.yaml,target=/config/delta-sharing-server-config.yaml  \
    --mount type=bind,source=<path>\config\core-site.xml,target=/opt/docker/conf/core-site.xml deltaio/delta-sharing-server:0.3.0 \
    -- --config /config/delta-sharing-server-config.yaml

- See Delta sharing.ipynb on how to retrieve data

Useful links:
https://blog.devgenius.io/delta-sharing-on-azure-a64a4fb8b0c2
https://github.com/delta-io/delta-sharing/blob/main/README.md
https://hadoop.apache.org/docs/stable/hadoop-azure/abfs.html#Authentication
https://github.com/delta-io/delta-sharing/blob/main/server/src/universal/conf/delta-sharing-server.yaml.template