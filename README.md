# golang-cli-tool

[![Build Status](https://travis-ci.org/Manuhmutua/golang-cli-tool.svg?branch=master)](https://travis-ci.org/Manuhmutua/golang-cli-tool)

The question required me to create a CLI that allows the user to list all the resources below:
- IAM users
- EC2 instances
- RDS instances
- S3 buckets
- ECR repos
- ECS clusters

The credentials are to be stored in a file on the user’s machine. The file is a simple text file in the following format:
```sh
AWS_ACCESS_KEY_ID = AKIAXXXXXXX
AWS_SECRET_ACCESS_KEY = XXXXXXXXXXX
AWS_REGION = xx_xxxxx
```

The application should take an input flag corresponding to the location of the credentials file, handle user input errors gracefully and provide a help menu.

## Running the application

The fastest and most convinient way to use this application is by using [docker](https://www.docker.com). You can also clone and build the repository in your local machine if you have AWS cli and Golang installed in your local environment. I'll only explain how to run the application using docker.
Make sure you have docker installed in your machine to continue.

### Building the application 

To use the application you have to build a docker image with the Dockerfile placed in the root directory of the project. 
You can use the Makefile like:
```sh
make build
```
You can also view availble commands in the make file using: (In this case we have only one command.)
```sh
make help
```
Alternertively you can build the image from the root folder of the application by running this command:
```sh
docker build . -t <Name of the Image you want to build>
```

That's it!

### Setting up the application 
 
To use the application, you have to first provide a configuration file as stated in the problem, add it to your application like: 
```sh
docker run -v "<Absolute path of file in your machine>:/home/sample.txt -it test/cli:latest apply -f /home/sample.txt
```
or 
```sh
docker run -v "<Absolute path of file in your machine>:/home/sample.txt -it <name of the image you build> apply -f /home/sample.txt
```
We are doing this to mount the file to our container so that we can be able to run the `apply -f/ --file` command.

For basic access to `gcli` commands, run the following:
```sh
alias gcli='docker run -it test/cli:latest'
```
or 
```sh
alias gcli='docker run -it <name of the image you build>'
```

You can now run commands like:
```sh
gcli help
```

### Using the application 

To list any of the required resources you should use the list command like:
```sh
gcli list ec2
```
 for more details run:
 ```sh
 gcli help list
 ```

 **NB: This application is not tested in a real aws environment**

<section class="compliance-report"><h2>Compliance Report- WIZ</h2><table border="1" cellpadding="5" style="table-layout: fixed;" width="100%"><tr><th>Compliance Status</th><th>Matches</th><th>Details</th></tr><tr><td style="width: 10%; text-align: center;"><p>🔴</p></td><td style="text-align: center;">1</td><td><details><summary>Show</summary><hr><b>Rule: </b>API Gateway stages access logging should be enabled<br/><b>Expected: </b>aws_api_gateway_deployment[this].stage_description should be set<br/><b>File Name: </b>aws/modules/api-gateway-v1/main.tf<br/><b>Line Number: </b>21<br/><b>Resource Name: </b>aws_api_gateway_deployment[this]<br/><hr/></hr></details></td></tr><tr><td style="width: 10%; text-align: center;"><p>🟠</p></td><td style="text-align: center;">2</td><td><details><summary>Show</summary><hr><b>Rule: </b>KMS key is scheduled for deletion<br/><b>Expected: </b>KMS key is scheduled for deletion (informational)<br/><b>File Name: </b>aws/modules/kms/main.tf<br/><b>Line Number: </b>1<br/><b>Resource Name: </b>aws_kms_key[this]<br/><hr/><b>Rule: </b>KMS key is scheduled for deletion<br/><b>Expected: </b>KMS key is scheduled for deletion (informational)<br/><b>File Name: </b>aws/modules/route53-zone/main.tf<br/><b>Line Number: </b>60<br/><b>Resource Name: </b>aws_kms_key[this]<br/><hr/></hr></details></td></tr><tr><td style="width: 10%; text-align: center;"><p>🟡</p></td><td style="text-align: center;">4</td><td><details><summary>Show</summary><hr><b>Rule: </b>Resource should be protected by Shield Advanced<br/><b>Expected: </b>aws_lb has shield advanced associated<br/><b>File Name: </b>aws/modules/alb/main.tf<br/><b>Line Number: </b>1<br/><b>Resource Name: </b>aws_lb[this]<br/><hr/><b>Rule: </b>Resource should be protected by Shield Advanced<br/><b>Expected: </b>aws_cloudfront_distribution has shield advanced associated<br/><b>File Name: </b>aws/modules/cloudfront/main.tf<br/><b>Line Number: </b>1<br/><b>Resource Name: </b>aws_cloudfront_distribution[this]<br/><hr/><b>Rule: </b>Resource should be protected by Shield Advanced<br/><b>Expected: </b>aws_route53_zone has shield advanced associated<br/><b>File Name: </b>aws/modules/route53-zone/main.tf<br/><b>Line Number: </b>1<br/><b>Resource Name: </b>aws_route53_zone[this]<br/><hr/><b>Rule: </b>Resource should be protected by Shield Advanced<br/><b>Expected: </b>aws_eip has shield advanced associated<br/><b>File Name: </b>aws/modules/vpc/main.tf<br/><b>Line Number: </b>297<br/><b>Resource Name: </b>aws_eip[nat]<br/><hr/></hr></details></td></tr></table></section>
