# Jenkins


![jenkins](https://user-images.githubusercontent.com/86292184/127503485-703d0f5f-1705-40fb-b4f5-94f4a77566a8.png)
<br> </br>
- -------------------------------------------------------
### Launch ami of instance
- Select ami you would like to launch 
- Select launch
- Go though instance set up steps 
<br> </br>
- When ssh back into instance, make sure to change the ssh key from
`root` to `ubuntu`
- make sure ot update it `sudo apt-get update -y`
<br> </br>
- -------------------------------------------------------


## Setting up Continuous Integration, CI, job
**Steps**
<br> </br>
- Click on **New Item**
- Enter a name, select **Freestyle project** and press okay
<br> </br>


#### General
- You will now be able to configure your job
	- Description is optional
- Select **Discard old builds** and type `3` in **Max # of builds to keep**
- Select **GigHub Project** and paste your github's HTTP of the repo
<br> </br>


#### Office 365 Connector
- Select **Restrict where this project can be run** and enter `sparta-ubuntu-node `
<br> </br>


#### Source Code Manager
- Select **Git**
- In **Repository URL** paste the repos SSH URL
- **Credentials**
		- Select **Add** -> **Jenkins**
		- Open **kind**  -> **SSH Username with private key**
		- Enter a username and select **private key** -> **Enter directly**
		- Enter private SSH key (can locate in .ssh file through gitbash)
		- change **Branches to build** -> `*/dev`
<br> </br>


#### Build Triggers
- Select **GitHub hook trigger for GITScm polling**
		- More on webhooks below
<br> </br>

#### Build environment 
- Select **Provide Node & npm bin/ folder to PATH**

#### Build
- Select **Add build step** -> **Execute shell**
- Add linux commands:
```
cd app
npm install 
npm test
```
<br> </br>


**FOLLOW ONLY AFTER YOU HAVE COMPLETED MERGE JOB**
- Open up CI job through **Configure**
- Under **Post-build Actions** -> **Build other project**
- Enter name of Merge job
- Select **Trigger only if build is stable**
<br> </br>

- --------------------------------------
### github webhooks
- In github repo, go to `settings` -> `webhooks`
- Add the jenkis ip + port +  `/github-webhook/`
<br> </br>


- -----------------------------------
## Setting up Merge job
<br> </br>
- Click on **New Item**
- Enter a name, select **Freestyle project** and press okay
<br> </br>

#### General

- Select **Discard old builds** and type `3` in **Max # of builds to keep**
- Select **GigHub Project** and paste your github's HTTP of the repo
<br> </br>


#### Office 365 Connector
- Select **Restrict where this project can be run** and enter `sparta-ubuntu-node `
<br> </br>


#### Source Code Manager
- Select **Git**
- In **Repository URL** paste the repos SSH URL
- **Credentials**
		- Select credential you just created
		- change **Branches to build** -> `*/dev`
<br> </br>


#### Build environment 
- Select **Provide Node & npm bin/ folder to PATH**
<br> </br>
#### Post-build Actions
- Select **Add post-build action** -> **Git publisher**
- Select **Push Only If Build Succeeds**
- **Branches** -> **ADD Branch**
		- **Branch to push**: main
		- **Target remote name**: origin

<br> </br>
**FOLLOW ONLY AFTER YOU HAVE COMPLETED DEPLOY JOB**
- Open up Merge job through **Configure**
- Under **Post-build Actions** -> **Build other project**
- Enter name of Deplot job
- Select **Trigger only if build is stable**
- Drag **Build other projects** under **Git Publisher**
<br> </br>
- -------------------------------------------
## Allowing jenkins access to EC2 instance
- Open aws Ec2 instances
- In **App instance** -> **Security**
- Select **Security group** link
- Select **Edit inbound rules**
- Select **Add rule**
		- Change **Type** -> **SSH**
		- Change **Source** -> **jenkins ip**
- Save changes

- -------------------------------------
## Setting up Deploy job
<br> </br>
- Click on **New Item**
- Enter a name, select **Freestyle project** and press okay
<br> </br>

#### General

- Select **Discard old builds** and type `3` in **Max # of builds to keep**
- Select **GigHub Project** and paste your github's HTTP of the repo
<br> </br>


#### Office 365 Connector
- Select **Restrict where this project can be run** and enter `sparta-ubuntu-node `
<br> </br>


#### Build environment 
- Select **Provide Node & npm bin/ folder to PATH**
- Select **SSH Agent** and select **eng89_decops.pem**
<br> </br>

#### Build
- Select **Add build steps** -> **Execute shell**
- Add below code:
```


ssh -o StrictHostKeyChecking=no ubuntu@ec2-public-app-ip.eu-west-1.compute.amazonaws.com << EOF

	export DB_HOST=mongodb://public.db.ip:27017/posts
     printenv DB_HOST
    
    cd jenkins_app
    cd app
    
    pm2 kill
    pm2 start app.js

EOF
```
- Replace **public-app-ip** with App instant public app ip (use dashes)
- Replace **public.db.ip** with DB instant public db ip (use dots)

<br> </br>
- ------------------------------------------------------
### Creating an ami of instances
- From lists of instances, select the instance
- Select `Action` 
- Select `Image and Templates`
- Select `create image`
- Name image
- Add tag
- Create Image
<br> </br>
- Once ami is created, click on the ami ID
**Never terminate your instance before the ami completes**
<br> </br>
- -------------------------------------
