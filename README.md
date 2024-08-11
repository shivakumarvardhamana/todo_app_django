CSVServer Assignment

This project demonstrates the deployment of a CSV server application using Docker and Docker Compose, with added metrics monitoring via Prometheus.
Project Structure

The project is divided into three parts:
- **Part-1**: Running the CSV Server Container
 - **The requirefile available in part-1 folder**
 - image: infracloudio/csvserver:latest

       docker run -d --name csvserver infracloudio/csvserver:latest

steate of The container exited with an error (Exited (1)) due to a missing file.

Error Troubleshooting:

  docker logs csvserver

    2024/08/10 17:44:59 error while reading the file "/csvserver/inputdata": open /csvserver/inputdata: no such file or directory

Generating the Required File:

    A script named gencsv.sh was used to generate the necessary input file (inputFile), which was then mounted into the container.

Running the Container with the Input File:

    docker run -d --name csvserver -p 9393:9300 -v "/root/inputFile:/csvserver/inputdata" infracloudio/csvserver:latest

The container is running successfully. 
kill the container need to be add env variable
Setting Environment Variables:

        docker run -d --name csvserver -p 9393:9300 -v "/root/inputFile:/csvserver/inputdata" -e CSVSERVER_BORDER=Orange infracloudio/csvserver:latest

The application is accessible at http://localhost:9393, displaying the title with an orange border.

Files Overview:
gencsv.sh : the shell script to generate the inputfile
part-1-log: the file is contain the logs of docker container
part-1-output: store the result of application csvserver
part-1-cmd: the cmd used to run the container with inputfile and enviroment variable

**Part 2**: Docker Compose Setup
- **The requirefile available in part-2 folder**


In this part, the Docker commands from Part 1 were converted into a docker-compose.yml file for easier management.

Environment Variables:
An environment file (csvserver.env) was created to store variables required by the Docker Compose file.

Docker Compose Configuration:
The docker-compose.yml file was created to automate the setup of the CSV server with the necessary configurations:



    docker-compose up -d

To stop the services, use:

   

        docker-compose down
 - Files Overview:
    - csvserver.env : file to store the env variable need to pass in docker-compose file to read as file
   - docker-compose: the file contain all steps to crete container with require details

**Part 3**: Adding Prometheus for Metrics Collection
- **The requirefile available in part-3 folder**
  
Prometheus was integrated to collect metrics from the CSV server.

Prometheus Configuration:
A prometheus.yml file was created to specify the target application (csvserver:9300) for metrics collection.

Docker Compose Setup:
The docker-compose.yml file was updated to include the Prometheus container (prom/prometheus:v2.45.2), with the prometheus.yml file mounted as a volume.

-Files Overview:
  - csvserver.env: Stores environment variables for the CSV server.
  - docker-compose.yml: Defines the CSV server and Prometheus configurations.
  - prometheus.yml: Specifies the Prometheus target application.
  - inputFile: The file mounted as a volume to the CSV server for data input.

Usage

To start the entire setup, run:



    docker-compose up -d

To stop the services, run:


    docker-compose down
