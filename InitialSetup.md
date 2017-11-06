# Initial Setup on Mac



This guide will show you how to setup the required software on your Mac before you are ready to create accounts in AWS using Terraform templates.

## Install Xcode
The Xcode app needs to be installed for Brew to be installed. First instsall this from either the AppStore or from the command line.
```sh
$ xcode-select --install
```

## Install Homebrew

Homebrew is the package manager for macOS which installs packages you need that Apple didn't install :)

```sh
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Terraform and Python

Run the following command to install Terraform and Python.

```sh
$ brew install python terraform
```
>Terraform is a tool for building, changing and versioning infrastructure safely and efficiently. Terraform builds Infrastructure as code and using template files to automate the process.   
>Terrform is similar to AWS CloudFormation however Terraform supports AWS, Google Cloud Plaftorm, Chef, Docker.   
>Terraform's file format is .tf.     
>We use Terraform to build, VPCs, Subnets, NACLs. Terraform can for example be used to build EC2 instances automatically.

## Point pip to Artifactory

```sh
$ cd ~
$ mkdir .pip
$ touch .pip/pip.conf
$ vi .pip/pip.conf
```
Add the config file to the pip.conf file. #pip.conf
```sh
[global]
extra-index-url = http://artifactory/artifactory/api/pypi/pypi-local/simple
trusted-host=artifactory
```

>```pip``` is the package manager for Pyhton and is the recommended tool for installing Python packages. When installing packages locally we must point pip to point to Artifactory (directory for our packages at Tableau) to download packages from. 

## Create the AWS Credentials file

```sh
$ cd ~
$ mkdir .aws/
$ touch .aws/credentials
```

>The CLI stores credentials specified with the ```aws configure``` in a local file named ```credentials``` in the folder named .aws in your local home directory.   

## Create the folder for your Terraform templates 
This folder will contain all the terraform templates which you will create locally and then eventually pushed to GitLab. This directory can be called anything you like and store in any location.  Below is an example to create 2 folders.

```sh
$ mkdir -p Code/aws
````

## Install AWS SAML
Run the following command to install AWS-SAML.

```sh
$ pip2 install aws-saml
```

Autenticate with AWS.

```sh
$ aws-saml
```
>This allows Tableau AWS Users to authenticate our OneLogin Service into there provisioned account Tableau IT Account.

>Once authenticated it will generate:   
>Using 123458 for OneLogin application ID   
>Getting SAML assertion from https://1pl5p01qv8.execute-api.us-west-2.amazonaws.com/prod/saml-assertion...   
>Hooray! We got a valid SAML assertion   

The keys and tokens will be added to the ```credentials``` file. To view this 
```sh
$ cat ~/.aws/credentials
```

## Install AWS Shell

```sh
$ pip2 install aws-shell
```

## Authenticate and connect to AWS CLI 

```sh
$ aws-shell -p saml
```
>This will generate and add keypairs in the AWS configuration file. 

```sh
$ aws-shell
```
## [Setup account_manager](https://gitlab.tableausoftware.com/AWS_Cloud/account_manager)

Setup

```sh
$ pip install -r requirements.txt
$ pip install -e .
```

### Getting Master Billing Credentials

Utilizing the aws-saml package, the CLI can download credentials from OneLogin. Run the following
command, and then follow the prompts to save the credentials.

```sh
$ aws_acct_mgr auth login
````

# Useful Links

| Plugin | README |
| ------ | ------ |
| AWS Config file | [http://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html] [PlDb] |
| What is Terraform? | [https://www.terraform.io/intro/index.html}] |
