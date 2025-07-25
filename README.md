# Build Docker image and push it to nexus and docker hub using jenkins (using freestyle service)

So In the repo we are going to see how to build the application and pushed it to their respective repository managemnt using jenkins
as we know jenkins is used to automate the build of docker image, pushing of docker image and deploying of the docker image on the server Now we will see how to build and push the docker image using a **freestyle** service in jenkins.

The demo project we are going to use to build and push to their respective repository managements is a java-maven-app
[check out the source code](https://github.com/Hemanth42d/java-maven-app-learning-jenkins.git)

## Building Docker image and pushing it to docker hub

1. Click on a new job in the side bar of jenkins DashBoard (I assume you already installed jenkins on server [if not](https://github.com/Hemanth42d/install-jenkins-on-server.git)
2. Give it a name and select freestyle job service and save it.
3. Will be redirected to the job page you just created and select Configure in the side bar
> Now click on git under source Code Managemant and provide required info like url, credentials, branch.
> One thing to remember is to push inside a docker hub we need to login to the docker hub without login we can't push and providing the credentials in shell script is a bad practice so set credentials in credientials tab or click on add credentials in the down of the selection of credential dropdown, Give **USERNAME PASSWORD** as variables
> Click on execute shell under the build steps
> ```bash
> docker build -t <username_docker-hub>/java-maven-app:1.0 .
> echo $PASSWORD | docker login -u $USERNAME --password-stdin 
> docker push <username_docker-hub>/java-maven-app:1.0
> ```
4. click on save and then you will go to page with the sidebar option Build Now click on it
5. After few seconds you will get a success message saying Build Success if not Build Failure you can check the console logs by clicking on the build number and then on console logs and check what went wrong.



## Building Docker image and pushing it to nexus

1. Click on a new job in the side bar of jenkins DashBoard (I assume you already installed jenkins on server [if not](https://github.com/Hemanth42d/install-jenkins-on-server.git)
2. I assue you also have installed nexus on your server [if not](https://github.com/Hemanth42d/nexus-download-on-server.git)
3. Give it a name and select freestyle job service and save it.
4. Will be redirected to the job page you just created and select Configure in the side bar
> Now click on git under source Code Managemant and provide required info like url, credentials, branch.
> One thing to remember is to push inside a nexus  we need to login to the nexus without login we can't push and providing the credentials in shell script is a bad practice so set credentials in credientials tab or click on add credentials in the down of the selection of credential dropdown, Give **USERNAME PASSWORD** as variables
> And also need to say docker that the ip address we are providing is ours so it can allow the insecure registrie by creating a file and configure it like
> Also create a repo in nexus of docker(self hosted) and configur it by allowing http protocol and use a port 8080 for communication
>```bash
>vim /var/run/deamon.json
># { "insecure-registrie":["<ip_address":8083]
># check if you can access it
>http://<ip_address>:8083/v2/ # it should respond like {"errors":[{"code":"UNAUTHORIZED","message":"access to the requested resource is not authorized","detail":null}]} it means we are not loged in
>docker login <ip_address>:8083 # then it will asks for the username and password and it should shows Login Succed!
>```
>Then go for the below step 
>Click on execute shell under the build steps
>```bash
>docker build -t <ip_address>:8083/java-maven-app:1.0 .
>echo $PASSWORD | docker login -u $USERNAME --password-stdin <ip_address>:8083
>docker push <ip_address>:8083/java-maven-app:1.0
>```
4. click on save and then you will go to page with the sidebar option Build Now, click on it
5. After few seconds you will get a success message saying Build Success if not Build Failure you can check the console logs by clicking on the build number and then on console logs and check what went wrong.

YEP! that's Done for this repository.
  
  
