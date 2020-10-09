# NRRD Site Previews
This repository provides the nginx config that provides the proxy for the preview sites.

# Setup 
The nginx web server runs on cloud.gov. The web server passes the requests to an AWS S3 bucket that stores the various sites for previews. 

We use CircleCI to build the branches and use a sync command to send the content to the AWS S3 bucket. The path is created using the name of the branch. 

### Web Server Setup
To add/update the web server use the following command
```
cf push nrrd-preview -b https://github.com/cloudfoundry/nginx-buildpack.git -m 10M
```
### AWS S3 Storage Setup
[Cloud.gov documentation](https://cloud.gov/docs/services/s3/)

Use cloud foundry command to create public bucket the AWS S3 Storage
```
cf create-service s3 basic-public <SERVICE_INSTANCE_NAME>
```
Enable website mode on the AWS S3 bucket using aws cli
```
aws s3 website s3://${BUCKET_NAME}/ --index-document index.html 
```
Bind the service to the web server app(nrrd-preview)
```
cf bind-service <APP_NAME> <SERVICE_INSTANCE_NAME>
cf restage <APP_NAME>
```







