							**Docker**
Steps:

1. FROM openjdk:17-alpine3.14  
2. RUN mkdir /app  
3. COPY auth-server/target/auth-server-0.0.1-SNAPSHOT.jar /app/target/auth-server-0.0.1-SNAPSHOT.jar  
4. WORKDIR /app  
5. CMD "java" "-jar" '/app/target/auth-server-0.0.1-SNAPSHOT.jar'


Docker commands for local terminal:

docker build -t springapi .
docker images
docker run -p 8000:8080 springapi

How to upload .jar in docker container?
[https://www.youtube.com/watch?v=3SNKdr3f9Io&ab_channel=Randomcode](https://www.youtube.com/watch?v=3SNKdr3f9Io&ab_channel=Randomcode)

Why is the .jar file named SpringApi.0.0.1.jar?
<artifactId>SpringApi</artifactId>
<version>0.0.1</version>


  

					**Jfrog**
Steps:
1. Create an account on jfrog
2. Default username=admin, password=password
3. Customize your password and username
4. Create repository based on your requirement (maven, gradle)
5. Click on libs-snapshot-local
6.  Then click on set me up
7. Copy the code after entering the password
8. Paste the code in pom.xml file outside the <dependencies>
9. Generate the settings.xm file, update the usernames and password and store it in .m2 folder 
10. New it is ready to deploy
11. In intellij, check the maven setting (user settings, local repository )locations should .m2
12. Click on ‘execute maven goal’
13.  use command for deploy ‘mvn deploy -DskipTests’ for directly creating the .jar and deploy that .jar on selected repository.

**