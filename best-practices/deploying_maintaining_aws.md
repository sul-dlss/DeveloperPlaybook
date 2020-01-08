# Deploying and Maintaining Infrastructure in AWS

## Installation

> brew install terraform@0.11

Note: Until our terraform source is upgraded to support version 0.12.x+, version 0.11.x is required.

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
