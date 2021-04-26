# cloudproj4-jch

This project is a demonstrates the serverless pipeline within AWS, connecting different services (DynamoDB,SQS,CloudWatch Timer,  within AWS utilizing AWS's lambda functions to create a sentiment analysis engine.

![ScreenShot](/assets/flowchart_proj4.jpg)
<p align="center"><i>Figure 1: flowchart of a AWS serverless pipeline</i></p>

## Overview of the tools
1. DynamoDB - Amazon's Amazon Web Services (AWS) equivalent to MongoDB. Is very easy to establish and create a database.
2. SQS - Amazon Simpue Queue Service is Amazon's message queuing service that integrates with distributed systems and serverless applications and is an easy way for a tester to ingest test cases into your system
3. AWS lambda - A serverless compute service that allows you to run code without the need of provisioning or managing servers, allowing one to tie different services together
4. CloudWatch Timer - A serverless trigger that one can use to set up automated runs in a data pipeline.
5. AWS Comprehend - An off-the-shelf natural language processing application that one can utilize.
6. S3 - AWS's cloud storage. Highly scalable.
7. SAM - AWS Serverless Application Model - an open-source framework that one can use via a command-line tool to build serverless applications within AWS.
8. Cloud9 - AWS's cloud-based integrated development environment (IDE) that allows you to write, run, and debug code from your browser.

## Set up
This serverless pipeline was developed within Cloud9. If one does not already have a cloud9 instance established, go to your AWS portal and search for "cloud9" using the search bar at the top. Create the instance.

![cloud9](/assets/cloud9_instance.png)
<p align="center"><i>Figure 2: Cloud9 instance landing page</i></p>

After initializing an instance, it is recommended that one create a new virtual environment within your IDE before beginning to code. In doing so, one can eliminate any potential issues with existing packages. Additionally, combining with a <code>Makefile</code> and a <code>requirement.txt</code> makes this process repeatable.

To establish a python virtual environment in cloud9, run the following commands:

```
$ python3 -m venv {name of environment}
$ source {name of environment}/bin/activate
```

## Using SAM to set up AWS lambda functions (SAM part 1)
SAM should be pre-installed into your AWS Cloud9 IDE. To build an AWS lambda function from the terminal, run the command"
```
$ sam init
```
A guided walkthrough will begin. For this function, select the options:
* "1 - AWS Quick Start Templates"
* "2 - Image (artifact is an image uploaded to an ECR image repository)"
* "4 - amazon/python3.8-base" *\*note that at the time of writing, python 3.8 was the latest version of python*
* Give your lambda function a name. For this project, the first lambda was named <code>Consumer</code>.
* Choose the "1 - Hello World Lambda Image Example" template.
After creation, a directory will be generated with the name that you gave. Within that directory will be a folder named <code>hello_world</code>, containing a file named <code>app.py</code>. Transfer the code contained in <code>consumer.py</code> from this repository's <code>pyscripts</code> folder to the related <code>app.py</code>.

There will be some editing steps needed to connect the dynamoDB, SQS, and output S3 buckets that will go on in these lambdas before deployment, but that will be performed later. At this stage, a second lambda function needs to be created following the same process above, but this time transfering the <code>producer.py</code> instead of the <code>consumer.py</code>.

What <code>producer.py</code> and <code>consumer.py</code> do is shown by the flowchart above:
* <code>producer.py</code> connects to the database to get a name, in this case a company name (For this example we use the names of [FANG](https://en.wikipedia.org/wiki/Big_Tech). It then uses CloudWatch to put requests into SQS to feed the 2nd lambda function.
* <code>consumer.py</code> recieves input from SQS and then uses the Wikipedia API to grab a chunk of text. It then takes that text and applies sentiment analysis using AWS Comprehend. After receiving the sentiment analysis payload, it stores the output to an S3 bucket.

## Creating the incoming database, SQS, and output S3 bucket
### DynamoDB
To create the database, goto DynamoDB from the AWS portal, select <code>Create table</code>, input a table name, and for this example give a primary key of <code>name</code>. To input objects into the table, select <code>Items</code> and then <code>Create item</code>. For this example, four strings were created:
- amazon
- apple
- facebook
- netflix

![dynamoDB](/assets/dynamoDB.png)
<p align="center"><i>Figure 3: a table in DynamoDBe</i></p>


## Connecting all the parts together

## Using SAM finish setting up AWS lambda fuctions (SAM part 2)

## Conclusion
