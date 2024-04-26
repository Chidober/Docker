Step 1: Download the Docker .tgz Archive
First, you'll need to download the docker-18.09.1.tgz file. Since my environment doesn't allow me to provide direct download links or perform downloads, you should obtain this file from the official Docker website or GitHub release page, ensuring it's the correct version (18.09.1) you're after.
wget https://download.docker.com/linux/static/stable/x86_64/docker-26.1.0.tgz
Step 2: Extract the Archive
Once downloaded, transfer the .tgz file to your SLES server and navigate to the directory where you've placed the file. Extract the contents using the tar command:

sh
Copy code
tar xzvf docker-18.09.1.tgz
This will extract Docker's binaries, such as dockerd (the Docker daemon), docker (the Docker client command), and others into a folder.

Step 3: Move Docker Binaries to a System Directory
After extracting the files, move the Docker binaries to a directory in your system's PATH to make them accessible as commands. A common choice is /usr/local/bin/:

sh
Copy code
sudo mv docker/* /usr/local/bin/
Step 4: Start Docker Daemon
Before starting the Docker daemon, you might want to ensure that no other Docker processes are running:

sh
Copy code
sudo systemctl stop docker
Now, you can start the Docker daemon manually:

sh
Copy code
sudo dockerd &
To run the Docker daemon in the background, you might want to use a more robust method for managing the service, such as creating a systemd service file.

Step 5: Create a systemd Service File (Optional but Recommended)
For Docker to start automatically at boot and to be manageable with systemd commands (start, stop, status, etc.), create a systemd service file:

Create a new file named docker.service in /etc/systemd/system/:

sh
Copy code
sudo nano /etc/systemd/system/docker.service
Paste the following content into the file:

ini
Copy code
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target firewalld.service
Wants=network-online.target

[Service]
Type=notify
ExecStart=/usr/local/bin/dockerd
ExecReload=/bin/kill -s HUP $MAINPID
TimeoutSec=0
RestartSec=2
Restart=always

[Install]
WantedBy=multi-user.target
Reload systemd to recognize the new service:

sh
Copy code
sudo systemctl daemon-reload
Start Docker and enable it to start at boot:

sh
Copy code
sudo systemctl start docker
sudo systemctl enable docker
Step 6: Verify Docker Installation
Check that Docker is correctly installed and running:

sh
Copy code
docker --version
And test running a Docker container:

sh
Copy code
sudo docker run hello-world
This method of installing Docker provides flexibility but requires manual management of updates and security patches. Always ensure that you're downloading Docker from a trusted source to avoid security risks.




Step 1: Locate the Binary
Ensure that the docker-compose-Linux-x86_64 file is in a directory where you have write permissions, or move it to such a directory if necessary. If you've just downloaded it, make sure it's in a convenient location, such as your home directory or /tmp.
wget https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64
Step 2: Make the Binary Executable
Change the permissions of the binary to make it executable. You can do this with the following command:

sh
Copy code
chmod +x docker-compose-Linux-x86_64
for all users
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
Step 3: Move the Binary to a Directory in Your PATH
Move the now-executable binary to a directory within your system’s PATH to allow it to be run from any location. A common choice is /usr/local/bin/, which is typically included in the system's PATH. You might also rename the binary to docker-compose for ease of use:

sh
Copy code
sudo mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
Step 4: Verify the Installation
To confirm that Docker Compose is correctly installed and accessible, run:

sh
Copy code
docker-compose --version
