# Circleci for CI/CD Automation

## Circleci Config

 - steps that i follow in jobs
   
   1. Installing Dependencies (node.js,aws-cli,pip,awsdependencies) 

   2. Installing Front-End (Application) Files

   3. Installing Back-End (API) Files 

   4. Building Front-End Files

   5. Building Back-End Files

   6. Deploying The Application

   7. Deploying The API
    
version: 2.1
orbs:
  node: circleci/node@4.1.0
  aws-cli: circleci/aws-cli@1.3.1
  browser-tools: circleci/browser-tools@1.1.0
jobs:
  build:
    docker:
      - image: cimg/node:15.0.1-browsers
    steps:
      - node/install
      - checkout
      - aws-cli/setup
      - browser-tools/install-browser-tools
      - run: |
          node --version
          java --version
          google-chrome --version
      - run:
         name: Install pip
         command: |
           sudo apt install python3-pip
      - run:
         name: Install AWS dependencies
         command: |
           sudo pip install awsebcli
      - run:
          name: Front-End Installing Files
          command: |
            npm run frontend:install
      - run:
          name: Back-End Installing Files
          command: |
            npm run backend:install
      - run:
          name: Front-End Building Files
          command: |
            npm run frontend:build
      - run:
          name: Back-End Building Files
          command: |
            npm run backend:build
      - run:
          name: Deploy Application
          command: |
            npm run frontend:deploy
      - run: 
          name: Deploy Code
          command: |
            npm run backend:deploy

 ### Before Using Cirlcecli
   
   we need first to create a folder at the root of the project called .circlecli, inside that folder make a new file and name it config.yml inside that file you can now write your orbs and jobs 
   that you need in order to make the deploy process automated


 - first you need to login at the circlecli with your github account at https://circleci.com/vcs-authorize/

 - then select the repository that you upload the project to it

 - then select Faster: Use the .circleci/config.yml in my repo

 - then type main at ` Enter Branch Name Input Box `  (make sure that when you uploaded the project to the repository at github that your branch was main branch)

 - after that click on Setup the Project

## Using the Circleci 

in order to do the automation process first we need to configure our environment variables

  1. at the left navigation bar select project

  2. at the project that you setup it u will find word called unfollow project and that will make you sure that its is your project 

  3. you will find 3 dots, click on them and choose project settings

  4. it will open new page, at the left navigation bar choose Environment Variables 

  5. after choosing it will open then you can click on the Add Environment Variables Button
  and add all your environments variables that you are using for you project

- now you are ready for the automation deployment process all you have to do is to write the circleclie orbs and jobs in order to do all the deployment stuff automatic 

..... But Hey wait there is another step you have to do to make the circlecli config file jobs run 

- Go to the udagram root folder then open the package.json then write the scripts you need to run the circleclie jobs

## Installation 

 1. first the .circlecli/config.yml file you need first to do the following steps:

  1. node/install

    2. checkout

     3. aws-cli/setup

       4. browser-tools/install-browser-tools

 2. to install them do the next:
    - run: |
          node --version
          java --version
          google-chrome --version
      - run:
         name: Install pip
         command: |
           sudo apt install python3-pip
      - run:
         name: Install AWS dependencies
         command: |
           sudo pip install awsebcli

  3. after that write your other jobs in the following order:

    first installing 

    second building 

    finally deploying

   4. Do that for both the udagram-api package.json scripts commands and udagram-front end like the next example 

    - run:
          name: Front-End Installing Files
          command: |
            npm run frontend:install
      - run:
          name: Back-End Installing Files
          command: |
            npm run backend:install
      - run:
          name: Front-End Building Files
          command: |
            npm run frontend:build
      - run:
          name: Back-End Building Files
          command: |
            npm run backend:build
      - run:
          name: Deploy Application
          command: |
            npm run frontend:deploy
      - run: 
          name: Deploy Code
          command: |
            npm run backend:deploy

   5. after that go to the pakcage.json file at the udagram root folder and create the scripts with the name of the jobs commands that you make in config.yml file then put the commands that you need to make the steps you made (install , build , deploy ) and connect them with the right ( udagram-frontend and  udagram-api ) package.json scripts like the following example

        "frontend:install": "cd udagram-frontend && npm install",
        "backend:install": "cd udagram-api && npm install"

   6. now all you have to do is to push the project again to the github repository that you connected with Circlecli and Baaam every thing are going to work automatically