Docker Python documentation:  
https://hub.docker.com/_/python  

1. Install docker into the main folder of your app via the terminal (no need to start venv):  

        $ brew install --cask docker
 
2. Create a profile on Docker Hub, then log into Docker Hub via the terminal (input your password when prompted):  

        $ docker login

3. Create the requirements file:  

        $ pip freeze > requirements.txt

*To run the app independently of Docker, install requirements and run flask in venv:  

        (venv) $ pip install -r requirements.txt
        (venv) $ flask run

4. Create Dockerfile:  

        $ touch Dockerfile

5. Populate Dockerfile with the following:  

The starter image can be a variant, here alpine (otherwise `FROM python:3` is enough)

        FROM python:3.9-alpine

        WORKDIR /usr/src/app

        # Install dependencies
        COPY requirements.txt ./
        RUN pip install --no-cache-dir -r requirements.txt

        # Install the app (copies everything form present dir on computer, to the container)
        COPY . .            

        # Needed with Flask to acces this port
        EXPOSE 5000

        # Start the app (has to be an array)
        CMD [ "flask", "run" ]

*The default is a linux file system, here we create a file app (COPY needs to be updated as well if this option is chosen):  

        RUN mkdir /app
        COPY . /app 

6. Create a Docker image via the terminal (the dot at the end means "in this folder") giving it a name for the Docker repository:

        $ docker build -t my-python-app .

7. Run the app on your computer with, giving it a running name:

        $ docker run -it --rm --name my-running-app my-python-app
