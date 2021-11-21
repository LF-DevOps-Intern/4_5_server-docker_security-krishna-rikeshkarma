# Install the Docker security tools and scan all the created images and containers and optimize the vulnerabilities.

- There are quite a few Docker Security tools, and for this task, we will be installing a tool called Docker Bench. This docker security tool can run an audit of our docker’s host machine to find any possible issues we might not even know about.
- According to Docker documentation, “the Docker Bench for Security is a script that checks for dozens of common best-practices around deploying Docker containers in production.”
- Now let’s install and run Docker Bench:
  - To install Docker Bench, we need to clone the latest version from the official GitHub repository, we won’t find this tool in the standard repositories. So we need to clone the Docker Bench repository with the following command:
    - `git clone https://github.com/docker/docker-bench-security.git`<br/>
  ![git clone docker bench](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno1/snapshots/git%20clone%20docker%20bench.png)
  - Now we need to configure the Docker daemon, by modifying the Docker daemon configuration file so that it can be accessed by Docker Bench. To open the configuration file use the following command:
    - `sudo nano /etc/docker/daemon.json`
    - And in file, we need to add the following lines:<br/>
  ![add lines to daemon json](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno1/snapshots/add%20lines%20to%20daemon%20json.png)
    - Now we can close the file after saving it.
  - For Docker Bench, we also need to install Auditd, which is the Linux userspace component of the Linux Auditing System which is responsible for writing audit records to the disk. We can use the following command to install this software:
    - `sudo apt-get install auditd -y`<br/>
  ![installing auditd](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno1/snapshots/installing%20auditd.png)
    - Now again we need to open Auditd configuration file so that it can work with Docker. Let’s use the following command:
      - `sudo nano /etc/audit/audit.rules`
    - And at the bottom of this file, add the following lines:<br/>
  ![add lines auditd rules](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno1/snapshots/add%20lines%20audit%20rules.png)
    - Now we can close the file after saving it.
  - Now let’s run out the first audit. But before that let’s restart Auditd and Docker using the following commands:
    - `sudo systemctl restart auditd`
    - `sudo systemctl restart docker`<br/>
  ![restart auditd and docker](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno1/snapshots/restart%20auditd%20and%20docker.png)
    - Now let’s navigate inside the repository we cloned earlier and run the following command:
      - `sudo ./docker-bench-security.sh`<br/>
  ![scanning with docker bench 1](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno1/snapshots/scanning%20with%20docker%20bench%201.png)<br/>
  ![scanning with docker bench 2](https://github.com/LF-DevOps-Intern/4_5_server-docker_security-krishna-rikeshkarma/blob/main/Qno1/snapshots/scanning%20with%20docker%20bench%202.png)
    - We can see a lot of information being reported. The ones we need to pay close attention to are the ones that are marked as [WARN] because they might be a security issue. In the end, we can also see the number of checks performed and our score.
    - We can also save the output to a file if we want to, for this, we can use the following command:
      - `sudo ./docker-bench-security.sh -l scan-results`
      - We can view the scanned filed later, if we need to using following command:
        - `less scan_results`
        - We will be warned that it may be a binary file, but it is still viewable.
- We can observe that there are alot of warnings. Let’s try optimizing:
  - We can find one warning that says:
    - [WARN] 4.5 - Ensure Content trust for Docker is Enabled (Automated)
  - We can optimize this warning by enabling content trust with the following command in the terminal:
    - `sudo echo "DOCKER_CONTENT_TRUST=1" | sudo tee -a /etc/environment`
  - Then we need to restart the Docker and the Content trust warning will not be shown.
