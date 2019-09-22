# Trivago CI/CD pipeline in AWS

There is no need to clone this repository to your local machine. Read below how to go about it. If very eager go straight to [prerequisities](#prerequisities).

The repository proposes a solution to the technical challenge Trivago has posted for an CI/CD Engineer in AWS. More about the challenge in this [local file](Instructions.md).

The solution builds a Docker container, clones this repository to the container, prepares the pipeline in AWS and executes a simple Jupyter Notebook.

After the Notebook executes, you should se a JSON output starting like this:

```bash
{'results': [{'success': True
```

This means the pipeline and execution of the Notebook went fine.

The next step in the automatic process is to delete all object in AWS that were created from the container.

## Prerequisities

You have to have an AWS account!

Download or copy the [DockerFile](https://github.com/markokole/trivago-cicd-pipeline-aws/blob/master/docker/DockerFile) to a folder on your machine.

Prepare the file *aws_cred.env* with AWS environment variables in the same folder as the DockerFile is. Below is the file structure:

```bash
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=
AWS_DEFAULT_OUTPUT=text
```

You should have the *DockerFile* and the *aws_cred.env* ready now.

## Build the Docker container

```bash
docker build . --tag=trivago-image --file DockerFile
```

## Using the Docker container

### Automatic execution

The below command will automatically run the Docker and execute everything:

* clone the repository
* create the pipeline in AWS
* upload the JSON files to S3
* execute Python Notebook
* run test on the Notebook's output in AWS
* remove all AWS services in the pipeline

```bash
docker run -e github='https://github.com/markokole/trivago-cicd-pipeline-aws.git' -e bucketname='trivago-s3bucket' --env-file "aws_cred.env" -it trivago-image
```

There should be no services in AWS left after the automated job is done.

## The trivago.tar.gz file

This file has all the source code needed to build the AWS pipeline and the Jupyter Notebook can be found there, too. This is the code from Trivago to work on the challenge.
