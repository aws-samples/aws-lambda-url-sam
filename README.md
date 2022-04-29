# Deploying and Testing AWS Lambda Function URL with IAM Authentication

## This file describe how to deploy a lambda function URL using AWS SAM and how to test it using Signature V4 requests. 

This project contains source code and supporting files for a serverless application that you can deploy with the SAM CLI. It includes the following files and folders.

- lambdas/ - folder contains lambda function code
- client/v4-signing-lambda-url.py - script to test the application
- template.yaml - A template that defines the application's AWS resources.

The application uses several AWS resources, including Lambda functions and an API Gateway API. These resources are defined in the `template.yaml` file in this project. You can update the template to add AWS resources through the same deployment process that updates your application code.

## Deploy the sample application

The Serverless Application Model Command Line Interface (SAM CLI) is an extension of the AWS CLI that adds functionality for building and testing Lambda applications. It uses Docker to run your functions in an Amazon Linux environment that matches Lambda. It can also emulate your application's build environment and API.

To use the SAM CLI, you need the following tools.

* SAM CLI - [Install the SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
* Node.js - [Install Node.js 10](https://nodejs.org/en/), including the NPM package management tool.
* Docker - [Install Docker community edition](https://hub.docker.com/search/?type=edition&offering=community)

To build and deploy your application for the first time, run the following in your shell:

```bash
sam build
sam deploy --guided
```

The first command will build the source of your application. The second command will package and deploy your application to AWS, with a series of prompts:

* **Stack Name**: name the stack "lambdaurl". This is the stack name that our client application will look for. The name of the stack to deploy to CloudFormation. This should be unique to your account and region, and a good starting point would be something matching your project name.
* **AWS Region**: The AWS region you want to deploy your app to.
* **Confirm changes before deploy**: If set to yes, any change sets will be shown to you before execution for manual review. If set to no, the AWS SAM CLI will automatically deploy application changes.
* **Allow SAM CLI IAM role creation**: Many AWS SAM templates, including this example, create AWS IAM roles required for the AWS Lambda function(s) included to access AWS services. By default, these are scoped down to minimum required permissions. To deploy an AWS CloudFormation stack which creates or modifies IAM roles, the `CAPABILITY_IAM` value for `capabilities` must be provided. If permission isn't provided through this prompt, to deploy this example you must explicitly pass `--capabilities CAPABILITY_IAM` to the `sam deploy` command.
* **Save arguments to samconfig.toml**: If set to yes, your choices will be saved to a configuration file inside the project, so that in the future you can just re-run `sam deploy` without parameters to deploy changes to your application.

## Check the deployment

* go to Lambda Console and click on lambdaurl function.

* you can see the lambda code

* click configuration / click function URL

* Auth type is AWS_IAM, this means that AWS user should be authenticated to access the lambda function

* You can see the URL of Lambda Function

* click the URL link and you will get the the message: {"Message":"Forbidden"}

## test the Lambda URL using a signature v4 requests

* make sure you have Python3.8+ installed. [Download and Install Python](https://www.python.org/downloads/)

Go back to command line, inside the cloned repository and access the client folder:

```bash
cd client
```

* Installing python dependencies	

make sure you have boto3 and requests dependencies installed:

```bash
pip install boto3
pip install requests
```

* running the client app	

```bash
python v4-signing-lambda-url.py
```

* if you get the below response, is because you successfully authenticated using AWS credentials to call lambda URL.

```bash
{"deviceID": "123456", "deviceName": "Laptop"} 
```

## Cleanup

To delete the sample application that you created, use the AWS CLI. Assuming you used your project name for the stack name, you can run the following:

```bash
aws cloudformation delete-stack --stack-name lambdaurl
```

## Resources

See the [AWS SAM developer guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) for an introduction to SAM specification, the SAM CLI, and serverless application concepts.

See the [AWS Lambda developer guide](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html) to learn more about AWS Lambda