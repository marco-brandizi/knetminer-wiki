_(Currently available only as a preview feature)_

It is now possible to spin up a new dedicated Knetminer instance with a custom OXL file using Docker, or to the cloud using AWS ElasticBeanStalk. Follow the simple instructions below to see how.

## Before starting

1. You will need to have your OXL file, your SemanticMotifs.txt, and any other files required by the configuration (see below) accessible via an HTTP URL, i.e. saved on a web server or in an S3 bucket. They must be publicly accessible for the duration of the deployment but can be hidden or deleted once deployment is complete
2. If using Docker, you will need to install Docker locally
3. If using AWS ElasticBeanStalk, you will need an AWS account. The instructions below assume you will deploy Knetminer using the ElasticBeanStalk command-line interface (eb-cli), but you can also achieve the same results through the AWS console web interface

## To install using Docker

1. Into an empty folder download the [Dockerfile](https://raw.githubusercontent.com/Rothamsted/knetminer/20181001_autodeploy/common/quickstart/Dockerfile)
2. Edit the downloaded Dockerfile and in the first ENV section at the top update the settings
  a. Note that this includes the branch of the knetminer Git repository to use, and in most circumstances this should be the same branch that you got the Dockerfile from in the first place (`20181001_autodeploy` in this example) otherwise behaviour could be unpredictable
  b. The defaults shown in the file will also work if you just want to try it and see - the example URLs it references are in a public S3 bucket in the Knetminer AWS account
3. On the command line inside the folder where you downloaded the Dockerfile, type:
      `docker image build --squash -t knetminer .`
4. It will take about ten minutes to build on an average machine. When built, you can run it by typing:
      `docker run -p8080:8080 -it --rm knetminer`
5. After waiting for it to start up you’ll be able to access Knetminer at `http://localhost:8080/client/`

## To install using AWS ElasticBeanStalk

1. Into an empty folder download two files:
  a. [Dockerfile](https://raw.githubusercontent.com/Rothamsted/knetminer/20181001_autodeploy/common/quickstart/Dockerfile)
  b. [Dockerrun.aws.json](https://raw.githubusercontent.com/Rothamsted/knetminer/20181001_autodeploy/common/quickstart/Dockerrun.aws.json)
2. Edit the downloaded Dockerfile and in the first ENV section at the top update the settings
  a. Note that this includes the branch of the knetminer Git repository to use, and in most circumstances this should be the same branch that you got the Dockerfile from in the first place (`20181001_autodeploy` in this example) otherwise behaviour could be unpredictable
  b. The defaults shown in the file will also work if you just want to try it and see - the example URLs it references are in a public S3 bucket in the Knetminer AWS account
3. If you are using the AWS ElasticBeanStalk web interface, zip the folder containing the two files into a single zip archive, then go to the AWS console to upload and deploy it (the type is Generic->Docker, and your source bundle is the zip file you just created)
4. If you are using the ElasticBeanStalk command-line method instead, continue by typing, within the folder containing the two files, the following commands and following the instructions it gives you:
     `eb init`
     `eb create`
5. After waiting about ten minutes for it to start up, scan the output of the commands for the details of the load balancer it has created, or otherwise go to the AWS console in your web browser and look for the load balancer details there. Make a note of the load balancer's hostname (public DNS)
6. You can now access Knetminer at: http://<your-load-balancer-hostname>/client/ 