# A CI/CD pipeline in AWS

[Link to the technical challenge](https://jobs.zealpath.com/m/case/detail/559).

*Text from the technical challenge in case the link does not work:*

AWS DevOps Engineer, Hotel Profiling
In this case scenario, you are in a team of data scientists and AWS engineers. In the past, data scientists would train models by pulling data to their local machine and training the model there, and then hand over the code to AWS engineers. The engineers then re-implement the model in AWS as a running data pipeline on a case-by-case basis, making sure that we have versioning, environments, etc.

This handover setup is not ideal because it makes the time to production longer and it requires additional planning and prioritisation. The team is motivated to change that by creating a platform, where data scientists can maintain the whole life cycle of their models as independently from the engineers as possible.

In this vision, a data scientist would push a model, typically in the form of a Jupyter notebook, to a repository as the start of a pipeline on the platform. The pipeline stores the output data on S3, so that it is ready to be shared with data consumers. All the current models are batch jobs that should run daily. In the model, data scientists may pull data from a Hadoop cluster, an AWS data lake in S3, or an AWS ElasticSearch cluster.

We want to make the platform easy to adopt for the data scientists. With proper guidance, instructions, and a demo, we expect the data scientists to adopt the platform relatively easy. In this case study, a solution architect in the team came up with a design on AWS. Part of the design is a CI/CD pipeline that takes a notebook in a Github repo and turns it into a Docker image.

## Challenge

### Task 1 – Dockerfile for running a notebook

Your point of entry is a Github repo with a notebook residing in it. See the attached notebook of this case (usp.ipynb). Create a Dockerfile that will be used to build the runnable Docker image with this model.

The data scientist has also provided a script called (check.py) to sanity check the output data. It needs to run after the notebook and notify us if the check failed. The output of this task should be a zip file containing a Dockerfile, and any additional code you write to carry out the tasks.

### Task 2 – CloudFormation for CI/CD pipeline

With your Dockerfile ready and added to the Github repo, design a CI/CD pipeline in CloudFormation that will package the notebook in Github into a Docker image (which can be deployed later on) and validate the resulting image.

*We would like to see:*

A CloudFormation template
A Makefile to deploy your CI/CD pipeline given the Github repo name as an input parameter

### Task 3 – Monitoring in CloudFormation

The build and the validation of the resulting Docker image should have appropriate monitoring in place in case of failure. Engineers would like to be informed in a short time of every successful or unsuccessful execution of the CI/CD pipeline by some sort of alarm.

*We would like to see:*

Monitoring resources in the CloudFormation template
