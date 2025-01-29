# DevOps Challenge - Week 2 - Day 2: NCAA Game Highlights With Docker



## Project Overview:

In this project, I intergrated docker with AWS mediaConvert to fetch NCAA game highlight from rapidapi, stored the fetched data in an S3 bucket then use the mediaConvert to improve the audio and visual quality of the game higlights.

## Prerequisites

- Rapidapi.com API Keys
- Docker Desktop for Windows PC
- AWS Account
- IAM Role/Permissions: Ensure the user or role running the script has the following permissions

## Key components include:

1. Amazon CLI
2. AWS MediaConvert
3. Amazon S3 
4. IAM
5. Docker
6. Python 

## STEPS TO FOLLOW:

## Step 1: Clone Github Repository

1. `git clone https://github.com/iamDayoDev/docker-ncaa-game-highlights`

2. change directory into the cloned directory 
```bash
cd src
```

## Step 2: Create an IAM role

- On the AWS console search "IAM"

- Click Roles -> Create Role

- For the Use Case enter "S3" and click next

- Under Add Permission search for AmazonS3FullAccess, AWSElementalMediaConvertFullAccess and AmazonEC2ContainerRegistryFullAccess, then click next

- Input a name for the role

- Select Create Role

- Find the role in the list and click, Under Trust relationships Edit the trust policy to this:
```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "ec2.amazonaws.com",
          "ecs-tasks.amazonaws.com",
          "mediaconvert.amazonaws.com"
        ],
        "AWS": "arn:aws:iam::<"your-account-id">:user/<"your-iam-user">"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
## Step 3: Configure AWS CLI

1. In the CLI (Command Line Interface), type
```bash
aws configure
```
2. Follow the prompt and input your Access key and Secret key

## Step 4: Create an S3 bucket
```bash
aws s3api create-bucket --bucket <your-bucket-name> --region us-east-1
```
To fetch your mediaConvert Endpoint, Use this command
```bash
aws mediaconvert describe-endpoints
```

## Step 5: Update your .env file
RapidAPI_KEY: Ensure that you have successfully created the account and select "Subscribe To Test" in the top left of the Sports Highlights API
AWS_ACCESS_KEY_ID=your_aws_access_key_id_here
AWS_SECRET_ACCESS_KEY=your_aws_secret_access_key_here
S3_BUCKET_NAME=your_S3_bucket_name_here
MEDIACONVERT_ENDPOINT=https://your_mediaconvert_endpoint_here.amazonaws.com

## Note: Leave the rest the way they are configured.

## Step 6: Build your Docker image 
1. In the CLI (Command Line Interface), type to Build the docker file run
```bash
docker build -t <name-of-your-choice> .
```
2. To run your docker container, use 
```bash
docker run --env-file .env <your-image-name/id>
```
3. Go back to go your console, navigate to your S3 bucket.
- Verify the video
- Verify the processed video 


### You have successfully created a video processing app using docker and mediaConvert with S3 as data storage.

### REMEMBER TO DELETE ALL THE RESOURCES DEPLOYED.