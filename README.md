
# CI/CD Pipeline for Django todo app deployment using Jenkins, Docker, and AWS

Our project entails deploying a Django todo app on an AWS EC2 instance using Docker and Jenkins for a streamlined deployment process.

![](https://github.com/Haris127/django-app-cloud-deployment/blob/master/Screenshots/cover.png)



## Prerequisites

Prior to commencing the project, it is essential to have the following prerequisites installed on your system.

- Account on **AWS**
- **Python** with Virtual environment & **Django** installed
- Todo App code (we will use code from this repository : [Django todo app](https://github.com/shreys7/django-todo))

## Step 1: Run Locally

Before proceeding with the deployment of the app on AWS, we will first conduct a local test run of the application.
- To begin, create a folder dedicated to the project. Then, open the folder in Visual Studio Code (VS Code). Once the folder is open in VS Code, navigate to the integrated terminal within VS Code for further commands and operations. Or you can use bash terminal instead of using the VS Code.
- Clone the repository.

```bash
    git clone https://github.com/shreys7/django-todo.git
```
![](https://github.com/Haris127/django-app-cloud-deployment/blob/master/Screenshots/1.JPG)

- Next, let's create a virtual environment named "myenv" for our app to ensure that it remains isolated and does not interfere with our system.

```bash
    python3 -m venv myenv
```

- Activate the vritual environment
For windows/VS Code

```bash
    myenv\Scripts\activate
```

For Linux/MacOS/Git Bash

```bash
    source myenv/bin/activate
```

- Change directory to the app directory

```bash
    cd django-todo
```

- To generate the necessary database migrations for our app, execute the following command within the project's terminal:

```bash
    python3 manage.py makemigrations
```

- To apply the migrations and update the database schema, run the following command in the terminal:

```bash
    python3 manage.py migrate
```

- To create an admin user for your Django app, use the following command in the terminal: 

```bash
    python3 manage.py createsuperuser
```

- You will be prompted to provide a username, password, and email address for the admin user. Please enter the required information as prompted.

- To start the server and run your Django app, execute the following command in the terminal:

```bash
    python3 manage.py runserver
```

- This command will start the development server, and you should see output indicating that the server is running. By default, the app will be accessible at http://localhost:8000/ in your web browser.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

## Step 2: Create requirement file

To create a requirements.txt file that captures all the project dependencies, execute the following command in the terminal:

```bash
    pip3 freeze > requirement.txt
```
- This command will generate a requirements.txt file containing the names and versions of all the Python packages installed in your virtual environment. This file is crucial for replicating the project's dependencies, making it easier to manage and reproduce the environment, especially when working with Docker.

## Step 3: Set up AWS EC2 instance

- Create an EC2 instance and establish an SSH connection to it from your terminal

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- Once you have successfully connected to the EC2 instance via SSH, your terminal will display a prompt similar to the following:

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- Now that we are connected to the EC2 instance, let's create a new directory named "django-todo-app" (you can choose a different name if you prefer). Use the following command to create the directory:

```bash
    sudo mkdir django-todo-app
```

- To navigate to the newly created directory "django-todo-app," use the following command:

```bash
    sudo cd django-todo-app
```

- To clone the Django todo app repository into the current directory on the EC2 instance, use the following command:

```bash
    sudo git clone https://github.com/shreys7/django-todo.git
```

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- Now change directory and move to the django-todo directory

```bash
    cd django-todo/
```

- To add your EC2 instance's public IP address to the allowed hosts in the **settings.py** file of your Django project.

```bash
    vi todoApp/settings.py
```

To modify the **ALLOWED_HOSTS** setting in the **settings.py** file, find the allowed hosts section and enter your public IP or '*' to allow all IP addresses.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- Please navigate to the EC2 dashboard, then go to the "Security Groups" section, and edit the inbound rules of the relevant security group to add a rule that allows traffic from everywhere on the specified port. Please refer to the provided screenshot for guidance.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- Now, in the terminal, proceed to install Docker on the EC2 instance.

```bash
    sudo apt install docker.io
```

## Step 4: Create dockerfile and run it

- Using vim create a Dockerfile

```bash
    vim Dockerfile
```

- Press "i" to enter insert mode, and then write the file as depicted in the provided screenshot.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

You can retrieve the version of Django from the **requirements.txt** file we created earlier by checking its contents.

- Press esc and use :wq and enter to save the file and exit vim.
- Now build the docker file we created

```bash
    sudo docker build . -t todo-app
```

- After obtaining the container ID, you can execute the container using the following command by providing the copied container ID or name.

```bash
    sudo docker run -d -p 8000:8000 todo-app
```

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- Now, in your web browser, paste your public IP address to check if the app is running. Please refer to the provided screenshot for reference.

Enter your public IP:8000. example : http://13.233.85.217:8000/

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

## Step 5: Install and setup Jenkins on your EC2 Instance

- Update your system

```bash
    sudo apt update
```

- Install java

```bash
    sudo apt install openjdk-11-jre
```

- To install Jenkins, simply copy the following commands, paste them one by one into your terminal, and execute them consecutively.

```bash
    curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \   /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

```bash
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \   https://pkg.jenkins.io/debian binary/ | sudo tee \   /etc/apt/sources.list.d/jenkins.list > /dev/null
```

```bash
    sudo apt-get update
```

```bash
    sudo apt-get install jenkins
```

- Start Jenkins with these commands

```bash
    sudo systemctl start jenkins
```
```bash
    sudo systemctl enable jenkins
```
```bash
    sudo systemctl status jenkins
```

- Please add an inbound rule to your security group, allowing traffic on port 8080, similar to how we added the rule for port 8000.
- Now, open Jenkins in your browser by entering the public IP address and the port number.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- Retrieve the password from the provided location and paste it here.

```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- Proceed with the installation by selecting the option to install the suggested plugins in Jenkins.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- Create and set up your admin user on the Jenkins homepage to complete the setup process.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- In your terminal, add Jenkins to the sudoers file to grant it sudo access, as illustrated in the provided screenshot.

```bash
    sudo visudo
```
Open the specified file using the provided command and add the following line to include Jenkins.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

## Step 6: Create a GitHub repository for the project and push the code from your local machine to the repository.

- Create a new GitHub repository with name you want.
- Copy the URL of the newly created repository, then return to the terminal of the EC2 instance and update the remote repository configuration to point to your new repository.

```bash
    git remote set-url origin https://github.com/haris127/djando-todo-app.git
```

- Add all files to staged

```bash
    git add .
```

- Commit all the files

```bash
    git commit -m "added server code"
```

- Push the code to repository

```bash
    git push origin develop
```

## Step 7: Integrate jenkins with GitHub

- Access the Jenkins instance on your EC2 instance, navigate to **"Manage Jenkins"** and then **"Configure System"**. Locate the section for GitHub servers and add your GitHub credentials there.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- Save it.

## Step 8: Deploy the app using Jenkins now

- Create a new freestyle project job named **"todo-app"**.
- In the source code management section, select Git, and paste the URL of your repository.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- In the **"Branches to build"** field, enter **"develop"** since we have pushed our code to the develop branch.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- In the build step, add an **"Execute shell"** build step and include the following commands.

```bash
    sudo docker build . -t todo-app
    sudo docker run -d -p 8000:8000 todo-app
```

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- Save the configuration and click on **"Build Now"** to execute the job.
- Now, you can check in another tab using the public IP and port number to see if the app is running successfully.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

- **Congratulations!** Your app is now up and running, and you have successfully completed this project.

## Contact Me

- If you encounter any issues, feel free to reach out to me at **harisjaleel@outlook.com** for assistance.