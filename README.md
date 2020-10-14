# Aim
To showcase jenkins ci/cd with aws lamba integration and code scanning

## Summary Implementation

## Infrastructure Setup
- Jenkins with nginx as frontend on port 80 in AWS EC2
- Github's repository with Jenkins integration with secure key
- Serverless Framework (Cloud Formation integration)

## CI/CD
- Jenkins Pipeline with Github webook to trigger Poll SCM
- Deploy lambda functions to IAM role s3 bucket
- TruffleHog Scanner via docker

## Build Pipeline
1. SCM Checkout
2. Running Pytest
3. Run serverless config and deploy
4. Scanning via TruffleHog with post success/failure
5. Deployment

## Installation
Detail installation is available [here](/docs/README.md)

