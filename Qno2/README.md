# Migrate the default working directory of docker i.e /var/lib/docker to /app/docker/.

- By default, Docker stores most of its data inside /var/lib/docker directory. So for this task, we need to migrate this default directory to /app/docker.
  - So now to change we have to sync or migrate files from the default directory to our preferred directory, let’s start by stopping Docker from running because making the changes while docker is running is certain to cause errors. We can use the following command to stop Docker:
    - `sudo systemctl stop docker.service`
    - `sudo systemctl stop docker.socket`<br/>
  ![stop docker service and socket](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno2/snapshots/stopping%20docker%20service%20and%20socket.png)
  - Firstly let’s copy our contents from /var/lib/docker to our new desired location i.e /app/docker/. Let’s create our desired directory using the following command:
    - `sudo mkdir -p /app/docker`
  - After we are done creating our desired directory, let’s use the rsync command to transfer the files from /var/lib/docker/ to /app/docker using the following command:
    - `sudo rsync -avxP /var/lib/docker/ /app/docker`
    - This might take some few minutes for completion.
  - Now we need to edit the Docker unit file in path /lib/systemd/system/docker.service. Here we need to enter the new location inside the file, this is the systemd file that relates to Docker. For this let’s use nano to edit the file. Use the following command:
    - `sudo nano /lib/systemd/system/docker.service`
    - Here we need to add our new desired location in ExecStart line by putting -g and the location of the new Docker directory, which will look like:
      - ExecStart=/usr/bin/dockerd -g /app/docker -H fd:// --containerd=/run/containerd/containerd.sock<br/>
  ![setting new desired location](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno2/snapshots/setting%20new%20desired%20location.png)
  - Lastly, let’s reload the systemd configuration for Docker, after making all these changes. To reload the system daemons let’s use the following command: 
    - `sudo systemctl daemon-reload`
    - And to finally start Docker use the following command:
      - `sudo systemctl start docker`<br/>
  ![starting docker daemon and docker](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno2/snapshots/staring%20docker%20daemon%20and%20docker.png)
  - Now to confirm if we have successfully migrated our default working directory of docker to /app/docker, we can use the ps command to make sure if the Docker service is utilizing our new directory using the following command:
    - `ps aux | grep -i docker | grep -v grep`<br/>
  ![showing docker using new directory](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno2/snapshots/showing%20docker%20using%20new%20directory.png)