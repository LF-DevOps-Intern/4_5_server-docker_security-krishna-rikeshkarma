# Provide the least privileges to each container and read-only access in mount volumes.

- To provide least privileges to a container, we need to add a line in the Docker file which will set a non root user node and will manage the privilege.
  - Let’s take node js first, in the Docker file of node js we need to add the following line so that it has non root user node. Check the snapshot:<br/>
  ![nodejs docker file](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno3/snapshots/nodejs%20Dockerfile.png)
  - If the container does not provide non root user, we can create it after installing the dependencies and moving the source files into the container. Then run the container as a new user.
- Alternatively we can also grant the least privileges to the already existing containers.
  -  Let’s take Nginx as an example. Firstly we need to remove the user directive on top of the nginx.conf file.
  - Then provide temporary pid to Nginx.<br/>
  ![nginx config](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno3/snapshots/nginx%20config.png)
  - And finally using the temporary paths to run the services uswgi, fastci, scfi as on snapshot below:<br/>
  ![services on temp path](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno3/snapshots/services%20on%20temp%20path.png)
- And finally to give read only permissions to the docker volumes:
  -  Read only permissions can be given to docker containers by setting the read-only flag in docker images volume section to to true. This flag makes sure that the container has read only access in the mounted volumes.