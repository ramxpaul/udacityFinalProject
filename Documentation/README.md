# Udagram Deployment
in this project we are going to upload and deploy the Udagram Project to the AWS Services we are going to upload and deploy both the front-end and the API 

this documentation gives you a good understanding about how you can deploy this application and all the information you will going to need it for configuring .

## Getting Started

1. Push the content of the udagram folder to a repository Github 
2. After that there is many steps you have to do :

- First we need to Deploy the udagram-frontend to AWS S3 Bucket, to do that we need to make few steps :
  1. you need to build the udagram-frontend so you can see there is a folder called www has been added to your udagram-frontend folder that folder we are going to use it for the deployment purpose 
  2. after that you can serve it if you want to 
  3. then now you have to make a script to deploy the udagram-frontend but there is few steps you have to do it before this step 
  4. you have to make a new folder at the root of the udagram-front end and call it bin
  5. then you have to make a file inside that folder and call it deploy.sh
  6. after you made the file go inside and write that code >> aws s3 cp --recursive --acl public-read ./www s3://your bucket name/
  7. after that now you can go to the package.json file of the udagram-frontend and make a script to deploy you can call it deploy , then type this code inside the script >> chmod +x ./bin/deploy.sh && ./bin/deploy.sh
  8. then to use all that stuff you made follow the instructions in the installation step

- Second we need to Deploy the udagram-api to AWS Elastic Beanstalk, to do that we need to make few 

first of all :
 - you need to make an .env file and put all the environment variables that you need to make the project run, you can find the environment variables that we need to run the project at the udagaram-api/src/config/config.ts
 
steps :
  1. you need to build the udagram-api so you can see there is a folder called www has been added to ur udagram-api folder and while building we are going to ziping that folder www to Archive.zip and put it inside the www folder 
  2. in order to do that we are going to make a script that can make all that stuff, to do that open the package.json file that existed at the udagram-api folder and make a script called build and put that code inside that script >> npm run clean && tsc && cp -rf src/config www/config && cp .npmrc www/.npmrc && cp package.json www/package.json && npm run archive || cd www && zip -r Archive.zip . && cd ..
  3. now after ziping we are going to deploy that Archive.zip but first we need to finish few steps in order to do that
  4. we need at first to make a connnection with the EB AWS service, in order to do that we need to make a script that will init the EB stalk Application that you are going to create, in order to do that go to package.json and make a script called ebinit and inside that script write that code >> eb init udagram-api2 --platform node.js --region us-east-1, after the init we will find a new folder called 
  .elasticbeanstalk created and inside it we will find a file called config.yml we are going to config that file after step 4 and 5 finished 
  5. now you have an application called udagram-api2 ready and connected, now we are going to make an environment to that application so we can deploy our code inside it, in order to do that go to package.json and create a script called ebcreate and inside that script type that code >> eb create --sample udagram-api-dev2
  6. before deploying we need to make few steps, first we need to figure out that we have an application called udagram-api2 and an environment inside it called udagram-api-dev2 we need to let the EB knows that we are going to deploy the file called Archive.zip inside that environment, in order to do that open .elasticbeanstalk folder then open config.yml, under environment-defaults type the next >> 
  deploy:
    artifact: www/Archive.zip
  7. now we can deploy the udagram-api code, we need to make two steps in order to do that,
   first is:
    we always need to be sure that we connected to our EB application and there is a .elasticbeanstalk folder with config.yml inside it so we have to use the EB init command .
   second is:
    we need to use the environment inside it so we are going to use the EB use command,
   third is: 
    after all that we now can deploy the file
  8. so in order to do all that we are going to create a script called deploy and inside that script type the next code >>  npm run ebinit && eb use udagram-api-dev2 && eb deploy udagram-api-dev2


### Dependencies

```
- Node v14.15.1 (LTS) or more recent

- npm 6.14.8 (LTS) or more recent

- AWS CLI v2

- Python v

- Pip v
 
- A RDS database running Postgres

- A S3 bucket for hosting interface

- A Elastic Beanstalk applicatoin for hosting code

```

### Installation
 
 > npm i 
 > npm init -y 
 > npm i yarn 
 > python -m pip install pip==18.0

* do the above npm i and npm init for udagram root project and udagram-api and udagram-frontend

- First for Deploying udagram-fronend we need to do the next:
  
  1. npm run build   or yarn build

  2. npm run start   or yarn start

  3. npm run deploy  or yarn deploy

- Second for Deploying udagram-api we needto do the next: 
  
  first of all :
  the Environment Variables that you are going to need are :
      POSTGRES_HOST
      DB_PORT
      PORT
      POSTGRES_PASSWORD
      POSTGRES_USERNAME
      RDS_DIALECT
      POSTGRES_DB
      AWS_REGION
      AWS_DEFAULT_REGION=
      URL
      AWS_ACCESS_KEY_ID
      AWS_SECRET_ACCESS_KEY
      JWT_SECRET

  1. npm run build    or  yarn build
  
  2. npm run ebinit   or  yarn ebinit

  3. npm run ebcreate or  yarn create

  4. npm run deploy   or  yarn deploy