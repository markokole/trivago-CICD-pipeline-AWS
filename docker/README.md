# Jupyter Trivago AWS Automation

*These instructions are for Docker on Windows 10*

## Build the Docker container

```bash
docker build . --tag=trivago-image --file docker/DockerFile
```

## Stepping into the Docker container

### Automatic execution

* The GitHub repository in question gets cloned
* Data files get uploaded to S3
* Database and two tables in Athena get created
* The Jupyter Notebook gets executed
* Results are sent to S3 (from the Notebook)

```bash
docker run -e github='https://github.com/markokole/trivago-cicd-pipeline-aws.git' --env-file "docker/aws_cred.env" -it trivago-image

```

### Manual execution or rollback on AWS

Manual execution is possible with a few changes:

* Comment out: *CMD /start-trivago.sh* in Dockerfile
* build the Docker again!
* Enter the Docker:

```bash
docker run -v ${PWD}:/local-git --env-file "docker/aws_cred.env" -it trivago-image
```

* If manually running the script: In the */start-trivago.sh* enter the GitHub repository link
* If running a rollback on AWS (cleaning up), execute:

```bash
./rollback_onAWS.sh
```

### Using Docker for Jupyter UI

* Create the container

```bash
docker run -itd --rm -p 8888:8888 --name trivago --hostname trivago -v ${PWD}:/local-git --env-file "docker/aws_cred.env" trivago-image
```

* Enter the container

```bash
docker exec -it trivago bash
```

* Run the Jupyter Service

```bash
/usr/local/share/anaconda3/bin/jupyter notebook --ip=0.0.0.0 --notebook-dir=/local-git --allow-root
```

Wrap it in *nohup* if you wish to run it in the background.

#######################

## AWS cloudformation

BUCKET_NAME=markokole-bucket

OLD:

```bash
aws cloudformation create-stack --stack-name mystack --template-body file://local-git/trivago_files/datalake/template.yml  --parameters ParameterKey=BucketName,ParameterValue=mystack-bucket
```
