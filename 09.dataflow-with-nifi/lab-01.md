# Big Data Ecosystem

## lab-01: NiFi basic

### Goals

- Become familiar with the NiFi UI
- Create basic NiFi dataflow
- Version our first flow with NiFi-Registry

### Instructions (less comfortable)

#### launch NiFi from docker image and create first flow

1) Run nifi docker image

```bash
docker run --name nifi-latest \
-p 8443:8443 \
-e NIFI_WEB_HTTPS_PORT='8443' \
-e SINGLE_USER_CREDENTIALS_USERNAME=<username> \
-e SINGLE_USER_CREDENTIALS_PASSWORD=<password> \
--mount type=bind,source=$(pwd),target=/shared \
-d \
apache/nifi:1.25.0
```

```bash
docker exec -it nifi-dsti /bin/bash
```

2) launch NiFi ui

```bash
./bin/nifi.sh start --wait-for-init=180
```

3) access ui here: https://localhost:8443/nifi/login
4) create a directory in your docker shared directory called `uploads`
5) create a directory in your docker shared directory called `downloads`
6) create a text file in the uploads directory.

Here we're going to create our first very simple flow consisting of only two processors to move a file from point A to point B. (Please note that this flow is for demo purposes only and purposefully introduces what can be considered a common "anti-pattern". More on this later in the labs) 

7) open the nifi ui and create your first process group by dragging a process group onto the main canvas
8) double click the process group to open
9) add a new processor to the canvas called `GetFile`
10) add a new processor to the canvas called `PutFile`
11) create a `success` connection between `GetFile` and `PutFile` (this is the only relationship type for the `GetFile` processor)
12) add two `Funnel`s to the canvas below the `PutFile` processor

Note that this is not the main use for funnels but provides a nice place holder for processor relationships until we've decided what we want to happen with the relationship. Additionally, it will provide a nice visual aid for ensuring our flows are functioning the way that they are intended.

13) add a failure relationship from `PutFile` processor to a funnel and a success relationship to the other funnel
14) configure the `GetFile` processor to "get" files from your 'uploads` directory
15) while configuring the `GetFile` processor it is important to note that the 'Keep Source File' setting defaults to 'false'. This means that once the processor has gotten the file, it will attempt to delete the origin file.  
for now we'll change this setting to true
16) configure the 'PutFile` processor to "put" files in your 'downloads' directory
17) right click on the `GetFile` processor and start it. You should soon see your file qued in the success relationship between GetFile and PutFile
18) right click on and start the `PutFile` processor
19) you should soon see a file qued in the success relationship between the PutFile processor and the funnel.

#### launch NiFi Registry and configure it to work with the NiFi instance

1) Run nifi-registry image.

```bash
docker run --name nifi-registry-lattest \
-p 18080:18080 \
-d \
apache/nifi-registry:latest
```
2) access nifi-registry: http://localhost:18080/nifi-registry
3) open settings (wrench icon located in the top right corner)
4) create `NEW BUCKET`
5) in a terminal run `docker inspect nifi-registry-latest` and make note of the "Gateway" ip.
6) open nifi UI and open  `Controller Settings` (accessed through the hamburger in the top right corner of the UI)
7) open the Registry Clients tab and add a new Registry Client (the + button)
8) edit the client to add the registry url (http://dockercontainer.gateway.ip.here:18080)
9) confirm that this worked by versioning the flow Get/Put flow that we created earlier