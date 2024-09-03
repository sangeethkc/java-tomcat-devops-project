# DevOps Project with Tomcat Server, Java, Git, GitHub, Jenkins, Docker

This readme file provides an overview of a DevOps project that utilizes various technologies such as Tomcat server, Java, Git, GitHub, Jenkins, and Docker.

## Project Overview

The goal of this project is to implement a continuous integration and deployment pipeline for a web application using the following tools and technologies:

- Tomcat server: A popular Java-based web server and servlet container.
- Java: The programming language used for developing the web application.
- Git: A distributed version control system for tracking changes in the source code.
- GitHub: A web-based hosting service for Git repositories, providing collaboration features.
- Jenkins: An open-source automation server for building, testing, and deploying software.
- Docker: A platform for containerizing applications and running them in isolated environments.

## Project Setup

To set up the project, follow these steps:

1. Install Tomcat server on your machine.
    - Add a new user with the necessary permissions to run the Tomcat server.
        ``` sudo useradd -r -m -U -d /opt/tomcat -s /bin/false tomcat```
    - Download the latest version of Tomcat server from the official website.(I use version 9.0.93 for this project)
        ``` wget -c https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.93/bin/apache-tomcat-9.0.93.tar.gz```
    - Create a directory for installing Tomcat.
        ``` sudo mkdir /opt/tomcat```
    - Extract the downloaded archive to a directory on your machine.
        ``` sudo tar -xf apache-tomcat-9.0.93.tar.gz -C /opt/tomcat --strip-components=1```
    - Create a symbolic link to the Tomcat installation directory.
        ``` sudo ln -s /opt/tomcat /opt/tomcat/updated```
    - Update the permissions of the Tomcat installation directory.
        ``` sudo chown -R tomcat: /opt/tomcat```
    - Update the permissions of the Tomcat scripts.
        ```sudo sh -c 'chmod +x /opt/tomcat/updated/bin/*.sh'```
    - Create a systemd service file for managing the Tomcat server.
        ``` sudo nano /etc/systemd/system/tomcat.service```
    - Add the following content to the service file:
    
                [Unit]
                Description=Apache Tomcat Web Application Container
                After=network.target

                [Service]
                Type=forking

                Environment="JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64"
                Environment="CATALINA_PID=/opt/tomcat/updated/temp/tomcat.pid"
                Environment="CATALINA_HOME=/opt/tomcat/updated/"
                Environment="CATALINA_BASE=/opt/tomcat/updated/"
                Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"
                Environment="JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom"

                ExecStart=/opt/tomcat/updated/bin/startup.sh
                ExecStop=/opt/tomcat/updated/bin/shutdown.sh

                User=tomcat
                Group=tomcat
                UMask=0007
                RestartSec=10
                Restart=always

                [Install]
                WantedBy=multi-user.target
    - Reload the systemd manager configuration.
        ``` sudo systemctl daemon-reload```
    - Start the Tomcat service and enable it to start on boot.
        ``` sudo systemctl start tomcat```
        ``` sudo systemctl enable tomcat```
    - Verify that the Tomcat server is running.
        ``` sudo systemctl status tomcat```
    - Open the necessary port in the firewall to allow access to the Tomcat server.
        ``` sudo ufw allow 8080/tcp```
    - Access the Tomcat server admin console in a web browser by navigating to `http://localhost:8080`.

2. Set up a Java development environment.
    - Update the system package repository by running the following commands:
        ``` sudo apt update```
    - Install the Java Development Kit (JDK) on your machine.
        ``` sudo apt install default-jdk```
    - Verify the installation by running the following command:
        ``` java -version```
    - Install Maven to manage Java projects and dependencies.
        ``` sudo apt install maven```
    - Verify the installation by running the following command:
        ``` mvn -version```

3. Create a Git repository for your project on GitHub.
4. Clone the repository to your local machine.
5. Develop your web application using Java.
    - Create a simple web application using Java servlets and JSP pages.
        Refer this video: [Sample Web Application](https://youtu.be/74I687FsITo?si=sxuquWMPtTODeViF).
6. Use Git to track changes in your source code.
7. Push your changes to the GitHub repository.
8. Set up Jenkins to automate the build and deployment process.
9. Configure Jenkins to build and deploy your application using Docker.

## Continuous Integration and Deployment

With the project set up, you can now benefit from continuous integration and deployment. Here's how it works:

1. Whenever changes are pushed to the GitHub repository, Jenkins detects the changes.
2. Jenkins automatically triggers a build process, compiling the Java code and creating a deployable artifact.
3. Jenkins then builds a Docker image containing the artifact and pushes it to a Docker registry.
4. Finally, Jenkins deploys the Docker image to the Tomcat server, making the updated application available.

## Conclusion

By leveraging the power of DevOps tools such as Tomcat server, Java, Git, GitHub, Jenkins, and Docker, you can streamline your development process and achieve continuous integration and deployment for your web application.
