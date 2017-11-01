# AWS Account Creation



Follow this guide for a step by step walkthrough for AWS account creation.
### Upgrading account_manager

Before creating an account upgrade account_manager

```sh
$ cd acconut_manager
$ git pull
$ pip2 install aws-acct-mgr --upgrade
```


### Create the Distribution Lists in Office 365
Log into [Office 365 Admin](https://portal.office.com/adminportal/home#/groups) - Create an In cloud Distribution List. Groups > Groups > Add a Group > Type: Distribution List

- aws-tableau-<group-name>@tableau.com

	Example:
	Group Name: AWS Tableau Product Internationalization
	Email: aws-tableau-sharing-insights@tableau.com

### Create AD Security Groups
Within AD under TSI Groups > OneLogin > AWS > Create three Security - Group Global Accounts. 

- _Account_Admins
- _Account_Read_Only
- _Account_Users

![Alt text](https://github.com/Cemito/aws/blob/master/adaccounts.png)

Go back into [Office 365 Admin](https://portal.office.com/adminportal/home#/groups) and add IT Cloud and The Team's DL as members to the DL.

### Create AWS Account
```sh
aws-acct-mgr account create --email aws-group-name@tableau.com --name "Group Name"
```

Example:
```sh
aws-acct-mgr account create --email aws-tableau-product-internationalization@tableau.com--name "AWS Tableau Product Internationalization"
```
This will bring up the SAML authentication;
Username will appear; hit enter
Enter password
Then select 0 [0] master-billing and then 0 again.

Get the ID for the account:```aws-acct-mgr account list ``` This will ID will be used in step in the terraform template.

### Updating AD Groups
Within AD add the Description to the three newly created groups along with these settings:

Example: AWS Account ID: 637144570937.
  - Under Members tab add IT Cloud to only _Accounts_Admin
  - Set Manager under the Managed By Tab and tick Manager can update list only on the _Accounts_Users account.

### Terraform Template Generate

>Terraform account creation example:
		
		aws-acct-mgr terraform generate \
		--account-id 684324928269 \
		--application "product-internationalization" \
		--group "aws-tableau-product-internationalization@tableau.com" \
		-dc 460 \
		-e test \
		-t "product-internationalization" \
		-r us-west-2 \
		--tf-path /Users/csalih/Code/aws/aws_account_Product-Internationalization/ public \
		--public-cidr "172.31.0.0/16"

>Notes:
>dc - is spend area. (Budget Code)
>e - is either prod, pre-prod and dev.
>private is always us-west-2. Direct Connect connects to us-west-2.

```cd```into the directory created from the terraform generate (dir set in --tf-path of terraform generate)

```cd```into the ```global/``` directory

    terraform init && terraform plan && terraform apply
> terraform init, initializes the repo. pulls down the aws provider & modules
> If an error run ```terraform init``` and ```terraform apply```

```cd``` out into root directory
    
    terraform init && terraform plan && terraform apply
    
> If an error run ```terraform init``` and ```terraform apply```

### Create Repo on GitLab
Create a repo folder/project on GitLab https://gitlab.tableausoftware.com/AWS_Cloud
- Visibility level: Internal 
- Project Name: aws_account_Product-Internationalization (Same as tf path in Terraform generate)
- Description: AWS Account for Product Internationalization

### Pushing to the Git Repo
```sh
$ git init
$ git remote add origin git@gitlab.tableausoftware:AWS_Cloud/aws_account_Product-Internationalization.git
$ git add.
$ git commit -m "Initial commit"
$ git push -u origin master
````
### Associate account to CloudCheckr 
    $ aws-acct-mgr cloudcheckr add-credential --aws-account-id 1234123123
>If you get an error saying account not found, try the same command a day later and it will with the confirmation message: ```{u'Message': u'OK', u'Code': 200}```

### OneLogin Mapings

Within OneLogin add arn line under List of SAML Identity Providers
- a. *Before adding the new line copy the whole list for backup*
- b. OneLogin > Apps > AWS > [Configurations](https://tableau.onelogin.com/apps/596729/edit/#configuration) and add the new arm: arn:aws:iam::637144570937:saml-provider/OneLogin and make sure you add a comma after the one you add and the last one has no comma. 
- c. Hit SAVE – NOT ENABLE!
- d. More Actions > Reapply provision mappings

Within OneLogin > Apps > AWS > Provisioning > [Entitlements](https://tableau.onelogin.com/apps/596729/edit/#provisioning) > Hit Refresh 5 times.

Within OneLogin create the new rule by mapping the 3 accounts under [Rules](https://tableau.onelogin.com/apps/596729/edit/#mappings). Then click New Rule
- Conditions will contain the AD account its mapping to beginning with 1L.

![Alt text](https://github.com/Cemito/aws/blob/master/1lmembers.png)
>Verification Checks: Check the AWS Multitab, check GitLab, Office 365 Admin and the CloudCheckr page. 
