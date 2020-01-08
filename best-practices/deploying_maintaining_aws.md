# Deploying and Maintaining Infrastructure in AWS

## Installation

> brew install terraform

> brew install vault

Verify that the vault version is >= 1.2

> vault --version

## Setup

### Request a developer login

Open a new [Operations Task](https://github.com/sul-dlss/operations-tasks/issues/new) issue. 

- Include your `sunet`
- Request being added to the development and staging `DevelopersRole` 
- Request being added to the production `ReadOnlyRole`
- Request a `Vault` account

### Setup local credentials & profiles

See instructions in [terraform-aws](https://github.com/sul-dlss/terraform-aws/wiki/AWS-DLSS-Dev-Env-Setup)

### Ensure required environment variables are set

- Set VAULT_ADDR: `export VAULT_ADDR=https://vault.sul.stanford.edu/`
- Set AWS_SECRET_ACCESS_KEY: `export AWS_SECRET_ACCESS_KEY=<YOUR SECRET ACCESS KEY>` 
- Set AWS_ACCESS_KEY_ID: `export AWS_ACCESS_KEY_ID=<YOUR ACCESS KEY ID>`

### Obtain a vault token

> vault login -method=userpass username=USERNAME password=PASSWORD

## General steps to deploy and update resources via terraform

Initialize all provider resources and modules

> terraform init

Verify changes and updates

> terraform plan

Apply changes and updates

> terraform apply

## Verify deployment

### ECS

When deploying new images to ECS, log into the AWS console, and verify that the task enters a steady state. This can take several minutes.

1. Login to AWS console and switch to the appropriate role
1. Navigate to ECS Services
1. Navigate into the application cluster
1. Click the service you are deploying
1. Click on the `Events` tab

You should see a series of events similar to:

```
service service-name has reached a steady state.
service service-name has stopped 1 running tasks: task 
service service-name has begun draining connections on 1 tasks.
service service-name deregistered 1 targets in target-group service-name-alb
service service-name registered 1 targets in target-group service-name-alb 
service service-name has started 1 tasks: task [task uuid].
```

If the service fails to reach a steady state, and another task is started, i.e. the above steps are continuously repeated, there was a problem starting the task. Click onto a task ID and then the `Logs` tab to see if anything is logged related to the failure.