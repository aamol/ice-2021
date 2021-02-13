##

#### Go to Command/Shell/terminal

#### Navigate to directory where we have cloned repo and go inside `app` Directory

   ```bash
   cd app/
   ```

#### 1) Lets run our first container - mongodb

   ```bash
   docker run -p 27017:27017 --name mongo -d mongo:latest
   ```

#### 2) Lets compile the app code on local

   ```bash
   ./mvnw clean install
   ```

#### 3) Lets run the compiled applicaton code on local without docker container

* Start application

   ```bash
   java -jar target/accessing-mongodb-data-rest-0.0.1-SNAPSHOT.jar
   ```

#### 4) Access application 

* Access Rest API interface of application via CURL command -  

   ```bash
   curl http://localhost:8080/people
   ```

* Now lets insert some data via application -

   ```bash
   curl -i -X POST -H "Content-Type:application/json" -d "{  \"firstName\" : \"Frodo\",  \"lastName\" : \"Baggins\" }" http://localhost:8080/people
   ```

* Lets again access application to see what is returned -

   ```bash
   curl http://localhost:8080/people
   ```

* Stop local running application -

   > press ctrl+c

* Now lets run our application in container -
  * First we will containerized this, create a container image for application
  * Lets Open DockerFile present inside app/ folder
  * For building image, lets run  -

   ```bash
   docker build -t k8s-demo-spring-mongo:1 .
   ```

  * For running this Image as Conatiner, run below commands -

   ```bash
   docker run --rm --name demo-app -p 8080:8080 k8s-demo-spring-mongo:1
   ```

  * what happen ?? Error ?? Why ?

  * Lets re-run again with some additional flags/parameter, stop current running container (press ctrl +c to stop)
  
   ```bash
   docker run --name demo-app --link mongo:mongoserver -e SPRING_DATA_MONGODB_HOST=mongoserver -p 8080:8080 k8s-demo-spring-mongo:1
   ```

  * --link is used to link one container with other, and container can talk to other using name defined in link
  * -e is used to pas Environment Variable inside the container  

  * Access Rest API interface of application via CURL command -  

   ```bash
   curl http://localhost:8080/people
   ```

  * Now lets insert some data via application -

   ```bash
   curl -i -X POST -H "Content-Type:application/json" -d "{  \"firstName\" : \"Frodo-2\",  \"lastName\" : \"Baggins\" }" http://localhost:8080/people
   ```

  * Lets again access application to see what is returned -

  ```bash
   curl http://localhost:8080/people
  ```

  ### Cleanup

  Lets cleanup our system, run below commands

   ```bash
   docker stop mongo demo-app
   docker rm -f mongo demo-app
   ```
  
  ### Troubleshooting
  
  We may face issue while compiling our code as shown below-
  Error: JAVA_HOME not found in your environment.
  
  To resolve it, we have to set the JAVA_HOME variable in our environment and for that we can follow the steps here- https://mkyong.com/java/how-to-set-java_home-on-windows-10/
  
