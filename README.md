# Learning
This is a documentation area for things I learn

Docker Images 
Building and Running a Python Flask App in a Docker Container
In this project, I worked on deploying a simple Python Flask application inside a Docker container, starting from the Ubuntu OS. Here's how I proceeded:

1. Pulling and Running Ubuntu OS Inside a Docker Container
To begin, I pulled the ubuntu image and started an interactive terminal session within the container. This allowed me to work in a fresh Ubuntu environment.

´´´docker run -p 8080:8080 -it ubuntu bash```

### Explanation:
-p 8080:8080: This maps the container's port 8080 to my local machine's port 8080, enabling access to the Flask app from the host browser.
-it: Interactive mode with a terminal session.
ubuntu: The base image for Ubuntu OS.
bash: Opens a Bash terminal inside the Ubuntu container.
Once inside the container, the prompt changed to:

graphql
Copy code
root@d80e61c125e1:#
2. Setting Up the Flask Environment
The next step was to install Python3 and Flask inside the container:

bash
Copy code
apt-get update                   # Update package information
apt-get install -y python3        # Install Python 3
apt-get install python3-pip       # Install pip (Python's package manager)
apt install python3-flask         # Install Flask via the package manager
Explanation:
The commands ensure that Python, pip, and Flask are installed in the container. Flask is a lightweight Python web framework ideal for developing web applications.
3. Creating the Flask App
Once the environment was set up, I created a simple Flask application by writing the app.py file. Using the cat command, I redirected the source code into /opt/app.py.

bash
Copy code
cat > /opt/app.py
I then pasted the following Python code:

python
Copy code
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
Explanation:
This Python script creates a Flask web app with two routes:
Route 1: The root path /, which returns a "Welcome!" message.
Route 2: A path /how are you which responds with "I am good, how about you?".
The app.run(host='0.0.0.0') ensures the app runs on all available IP addresses, allowing access from outside the container.
4. Running the Flask App
To run the Flask app, I used the following command:

bash
Copy code
cd /opt
FLASK_APP=app.py flask run --host=0.0.0.0
Explanation:
cd /opt: Changes the directory to where app.py is located.
FLASK_APP=app.py: Sets the app.py as the Flask app to run.
flask run --host=0.0.0.0: Runs the Flask server, making it accessible via the mapped ports.
Once the app is running, I can visit http://localhost:8080/ on my host machine's browser, and the "Welcome!" message should appear. Visiting http://localhost:8080/how are you will display the message: "I am good, how about you?"


