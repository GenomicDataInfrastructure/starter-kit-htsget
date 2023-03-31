# starter-kit-htsget

## Description
The docker compose file contains the following services:
1. Minio S3 backend (for file storage)
1. Script to add data to S3
1. Htsget server
1. Samtools and Htsget client for downloading files

In order to get the services up and running, use
```sh
docker compose up
```
from the root folder of the project.

Once the loading is finished, you can access the minio S3 in the browser at `localhost:9000`. The credentials, which can be found in the `docker-compose.yml` file, are `access:secretkey`. Then you should be able to see the files from local `data` folder (on the root of the project) in the `archive` bucket on the browser.

# Data access
There are different ways to access the data uploaded in the minio S3. For this implementation, only the **full file download** is possible.

## Alternative 1: Download a file using samtools 
In order to download the file using samtools, execute into the samtools container using:
```sh
docker exec -it samtools /bin/bash
```

### In bam format
Then run
```sh
samtools view -b http://server:3000/reads/s3/NA12878_20k_b37.bam > NA12878_20k_b37.bam
```
You should then be able to see the `NA12878_20k_b37.bam` on the folder you ran the command.

### In sam format
Then run
```sh
samtools view http://server:3000/reads/s3/NA12878_20k_b37.bam > NA12878_20k_b37.sam
```
You should then be able to see the `NA12878_20k_b37.sam` on the folder you ran the command.

## Alternative 2: Download a file using the htsget client

In order to download the file using htsget-client, execute into the htsget-client container using:
```sh
docker exec -it htsget-client /bin/bash
```
then run
```sh
htsget -v http://server:3000/reads/s3/NA12878.bam --output NA12878.bam
```
You should then be able to see the `NA12878.bam` on the folder you ran the command.

## Alternative 3: Using curl
In case you want to download the files locally (not using a container), you need to add the `s3-archive` to your `/etc/hosts` file. That can be done using
```sh
sudo vim /etc/hosts
```
and add
`127.0.0.1 s3-archive` in the file.

Then you should be able to run any of the above commands from the host machine, after installing the respective software (samtools or htsget).

# Notes
1. The implementation does not contain the authentication part, which is currently under development.
2. It is only possible to request entire files. The partial request is also under development.
