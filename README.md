# Aim
To showcase jenkins ci/cd with aws lamba integration and code scanning

## Summary Implementation
Here's the setup details:
 - Jenkins: http://ec2-18-191-188-223.us-east-2.compute.amazonaws.com/ (Can register as a user)
 - Github: https://github.com/chessz/aws-lambda-jenkins

### Infrastructure Setup
- Jenkins with nginx as frontend on port 80 in AWS EC2
- Github's repository with Jenkins integration with secure key
- Serverless Framework (Cloud Formation integration)

### CI/CD
- Jenkins Pipeline with Github webook to trigger Poll SCM
  - Install additional plugins
  - Setup Credentials
  - Create Build via Pipeline (Jenkinsfile)
- Deploy lambda functions to IAM role s3 bucket
  - Assign Lambda deployment IAM role
  - Using serverless framework via AWS
- TruffleHog Scanner via docker

### Build Pipeline
1. SCM Checkout
2. Running Pytest
3. Run serverless config and deploy
4. Scanning via TruffleHog with post success/failure
5. Deployment

### Installation
Detail installation is available [here](/docs/README.md)
