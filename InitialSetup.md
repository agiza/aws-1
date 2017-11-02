# Initial Setup on Mac



This guide will show you how to setup the required software on your Mac before you are ready to create accounts in AWS using Terraform templates.

## Install Xcode
The Xcode app needs to be installed for Brew to be installed. 
```sh
xcode-select --install
```

## Install Homebrew

Homebrew is the package manager for macOS which installs packages you need that Apple didn't install :)

```sh
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Terraform and Python

Run the following command to install Terraform and Python.

```sh
$ python terraform
```

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


## Install AWS SAML
Run the following command to install AWS-SAML.

```sh
$ pip2 install aws-saml
```
>This script allows Tableau AWS Users to authenticate our OneLogin Service into there provisioned account Tableau IT Account.

# Useful Links

| Plugin | README |
| ------ | ------ |
| AWS Config file | [http://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html] [PlDb] |
