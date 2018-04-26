# OVERVIEW:
We would like see a solution to run the 'notes' application using docker with CouchDB as its backend, the solution provided should enable us to run the 'notes' application as a container along with CouchDB on our machine. Please provide any supporting documentation

# RUNNIING THE NOTES APPLICATION WITH DOCKER:

## Repository’s

-	Dokcerhub: https://hub.docker.com/u/dawsmac/
o	This is hosting my Automated image of the frontend application “notes”
-	GitHub: https://github.com/dawsmac/mldfe
o	I have uploaded my Dockerfile to the Github for reviewing

To build the backend database server as a container “Couchdb” I chose to use the official apache image and to keep consistency “Version control” I have selected 1.7.0.
From your local machine that is running Docker, please run the following commands

docker pull couchdb:1.7.0

Once this has been pulled we will need to start up the container:

docker run -p 5984:5984 -d couchdb:1.7.0

To see if the container is running, run the below:

docker ps

Output should look close to the below

CONTAINER ID	IMAGE Name	COMMAND	CREATED	STATUS	PORTS	NAME
0a9f26c59860	dawsmac/mldfe:latest	"java -jar /opt/MLD/…"	12 minutes ago	Up 1 minutes      	0.0.0.0:8080->8080/tcp  	hungry_lalande
Next, we need to test and make sure that coucdb is accessible from our local machine, got to the below URL.
http://<machineip>:5984

Output should look close to the below:

{"couchdb":"Welcome","uuid":"8c5b28890084a2c5aa223ea1edd8c101","version":"1.7.0","vendor":{"version":"1.7.0","name":"The Apache Software Foundation"}}

As you will be running this from your local machine a new DNS record will be needed, as the container running the notes application references the following DNS record “http://couchdb:5984/”.

DNS record: couchdb  >> local machineip

Now to confirm the database backend is accessible via DNS:

http://couchdb:5984/

Again, the output should look close to the below:

{"couchdb":"Welcome","uuid":"8c5b28890084a2c5aa223ea1edd8c101","version":"1.7.0","vendor":{"version":"1.7.0","name":"The Apache Software Foundation"}}

Next we need to pull the frontend server as a container “mldfe“:

docker pull dawsmac/mldfe

Once this has been pulled we will need to start up the container:

docker run -p 8080:8080 -d dawsmac/mldfe:latest

We can now confirm both containers are running, run the below:

docker ps

Output should look close to the below:

CONTAINER ID	IMAGE Name	COMMAND	CREATED	STATUS	PORTS	NAME
0a9f26c59860	dawsmac/mldfe:latest	"java -jar /opt/MLD/…"	13 minutes ago	Up 13 minutes      	0.0.0.0:8080->8080/tcp  	hungry_lalande
25e401032d6b	couchdb:1.7.0	"tini -- /docker-ent…" 	13 minutes ago	Up 13 minutes      	0.0.0.0:5984->5984/tcp  	elastic_lamport

If all has gone well you should be able to access the below link:

http://<machinesip>:8080/swagger-ui.html

# SUMMARY:
I built the application locally 1st only using the backend database as a container, this enabled me to troubleshoot any issues that I came across with the notes application, at the same time enabling to me understand how the application worked. I quick discovered that notes had been coded with a DNS record, hence why I have added a step in the deployment guide around this. After I had successfully tested this I was able to move onto building the front-end into a container. I then applied a white-box approach for testing the application, make sure that I was getting the same results as with the local deployment.
