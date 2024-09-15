# Docker
## Mastering Docker Workflows: Building and Running a Flask Application in Containers
Sample: Building and Running a Python Flask App in a Docker Container. 
In this project, I worked on deploying a simple Python Flask application inside a Docker container, starting from the Ubuntu OS. This is in two parts, which involves performing the Docker steps manually, and then writing a Dockerfile to do the same for reproducible runs.
The goal of the Docker file is to do the same tasks so that they can be reusable in any environment without the manual work or issues with unified deployments. 

Here's how I proceeded: Within my Bash Terminal

## 1. Pulling and running an Ubuntu OS Image inside a Docker Container
To begin, I pulled the ubuntu image and started an interactive terminal session within the container. This allowed me to work in a fresh Ubuntu environment.
```
docker run -p 8080:8080 -it ubuntu bash
```
### Explanation:
```-p 8080:8080``` This maps the container's port 8080 to my local machine's port 8080, enabling access to the Flask app from the host browser.
```-it``` Interactive mode with a terminal session.
```ubuntu```: The base image for Ubuntu OS.
```bash```: Opens a Bash terminal inside the Ubuntu container.
Once inside the container, the prompt changed to:
```root@d80e61c125e1:#```

## 2. Setting up the flask environment
The next step was to install Python3 and Flask inside the container:
```
apt-get update                   # Update package information
apt-get install -y python3       # Install Python 3
apt-get install python3-pip      # Install pip (Python's package manager)
apt install python3-flask        # Install Flask via the package manager
``` 
Explanation:
The commands ensure that Python, pip, and Flask are installed in the container. Flask is a lightweight Python web framework ideal for developing web applications.
## 3. Creating the flask app
Once the environment was set up, I created a simple Flask application by writing the app.py file. Using the cat command, I redirected the source code into /opt/app.py.

```cat > /opt/app.py```
This opens an editor so you can paste your source code, I pasted following Python code:
```
import os
from flask import Flask

app = Flask(__name__)

@app.route("/")
def main():
    return "Welcome!"

@app.route('/how are you')
def hello():
    return 'I am good, how about you?'

if __name__ == "__main__":
    app.run(host='0.0.0.0')
```
### Explanation:
This Python script creates a Flask web app with two routes:
Route 1: The root path /, which returns a "Welcome!" message.
Route 2: A path /how are you which responds with "I am good, how about you?".
The ``` app.run(host='0.0.0.0')``` ensures the app runs on all available IP addresses, allowing access from outside the container.

## 4. Running the Flask app
To run the Flask app, i changed the directory to where i had my app ```cd /opt```I then used the following command:
```
FLASK_APP=app.py flask run --host=0.0.0.0
```
###Explanation:
cd /opt: Changes the directory to where app.py is located.
FLASK_APP=app.py: Sets the app.py as the Flask app to run.
flask run --host=0.0.0.0: Runs the Flask server, making it accessible via the mapped ports.
Once the app is running, I can visit http://localhost:8080/ on my host machine's browser, and the "Welcome!" message should appear. Visiting http://localhost:8080/how are you will display the message: "I am good, how about you?"

## 5. Stopping and restarting the Container
When you want to stop the Flask development server (or any container running a process), press Ctrl+C in the terminal where the server is running. This will gracefully stop the server.

Alternatively, you can stop the container itself:
```
docker stop <container_id>
```
Where ```<container_id>``` is the unique ID of the running container.

### Restarting the Container
Once the container is stopped, you can restart it by running:

```docker start <container_id>```
This will restart the container and continue running any previously specified processes.

If you need to access the container's terminal again after starting it:

```
docker exec -it <container_id> /bin/bash
```
This will give you a shell session inside the running container. However in my case i encountered an error.

## 6 Accessing the Docker Container in windows differently due to path handling
Initially, I encountered issues accessing the container shell using docker exec with the bash shell. On Windows, this is common because paths are treated differently than on Linux-based systems.

I tried running:
```
docker exec -it d80 /bin/bash```
But received the following error:
```
OCI runtime exec failed: exec: "C:/Program Files/Git/usr/bin/bash": stat C:/Program Files/Git/usr/bin/bash: no such file or directory: unknown```

### Solution: Using sh Instead of bash:
The solution was to use sh, which is a more basic shell that comes pre-installed in most containers. Because I was using Git Bash on Windows, I also had to use the path format //bin//sh:

``` docker exec -it d80 //bin//sh
```
This successfully opened a shell inside the container.
Why This Works:
Path Handling in Windows: When using Git Bash or CMD on Windows, the way paths are passed to Docker can cause issues. The //bin//sh path works around this by providing a compatible path format.
Use of sh Instead of bash: Some containers may not have bash installed, especially lightweight containers. sh is a basic shell that is included by default in most Unix-like systems, making it more reliable in such cases.

### Now I can rerun my flask app like I did before in step 4. 
Now that the steps are understood manually, its a great idea to write this in a Docker file so you can execute this faster everytime required.

## 7 Conclusion: Dockerfile for Reproducibility
The transition from manually setting up the environment to using a Dockerfile for automated builds has made the deployment process far more efficient. This approach ensures that the application can be deployed in any environment with minimal setup, reinforcing Docker’s core benefits of portability and consistency.


## Potential Expansion: Automated Deployment
In the future, this setup could be further automated using GitHub Actions or other CI/CD tools to trigger builds and deployments automatically when changes are pushed to the repository. 


