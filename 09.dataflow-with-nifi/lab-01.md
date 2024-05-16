# Big Data Ecosystem

## Lab \<lab number>: NiFi basic

### Goals

- Create basic NiFi ELT dataflow

### Instructions (less comfortable)

#### launch NiFi from docker image

```bash
docker pull apache/nifi:1.25.0
```

```bash
docker run --name nifi-dsti \
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

##### launch NiFi ui

```bash
./bin/nifi.sh start --wait-for-init=180
```

could be access here: https://localhost:8443/nifi/login

#### explore processors - ListFile & FetchFile
- config of processor
- why GetFile is not robust
- check the memory for ListFile + FetchFile vs. GetFile

#### explore processors - PutEmail
- config of processor

#### explore processors - UpdateRecord
- config of processor

#### explore processors - PutHDFS
- config of processor
- copy core-site.xml, hdfs-site.xml to container

#### explore processors - DeleteHDFS
- config of processor

#### explore processors - TailFile (log)

### Instructions (more comfortable)

#### explore processors - listSFTP & FetchSFTP

#### explore processors - PutIceberg

#### explore processors - 
