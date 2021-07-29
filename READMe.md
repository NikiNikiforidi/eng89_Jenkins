# Jenkins


![jenkins](https://user-images.githubusercontent.com/86292184/127503485-703d0f5f-1705-40fb-b4f5-94f4a77566a8.png)



## Setting up jenkins jobs.
**Steps**
<br> </br>
- Click on `New Item`
- Enter a name, select `Freestyle project` and press okay
<br> </br>
- You will now be able to configure your job
	- Description is optional
- Select `Discard old builds` and type 3 in `Max # of builds to keep `
- In BUILD section, open `Add build step` and select `Execute shell`
- Add some linus commands, for example `uname -a`, `pwd`, `ls -a` etc.
- Finish with `Apply` and `ok`
<br> </br>
- -----------------------------------
### Build and test project success

- On Jenkins dashbaord, select your job
- Select `Build Now` to build your job
- Under `Build History` every job you run will be displayed
- Select the latest build and select `Console Output` to check if build was  a 	SUCCESS
<br> </br>
- -----------------------------------
### Create pipeline between project

- Create and build a new job following the steps above.
- Make sure both jobs pass the build
<br> </br>
- Open the main job and select `Configure` 
- Scroll down to section POST-BUILD ACTIONS and open `Add post-build action`
- Select `Build other projects` 
- Add the name of your second job to `Projects to build`
- Select option `Trigger only if build is stable`
- Apply and save changes	
<br> </br>
- Now when you build your main job, all your conneted job will build also
<br> </br>
- ---------------------------------------
### Connecting jenkins to github
<br> </br>
**Make sure your deploy key is set to allow write**
<br> </br>
**CREATE MAIN JOB**
- Create new build and add `max # of builds to keep` to 3
- Select `GitHub project` and add you github repos http
- Section OFFICE 365 CONNECTOR select `Restrict where this project can be run` and add `sparta-ubuntu-node`
- Section SOURCE CODE MANAGER select git.
	- In `Reposiitory` add repo's ssh
	- Add new `Credentials` -> jenkins
	- Follow images set up and add PRIVATE key 
	![Capture](https://user-images.githubusercontent.com/86292184/127504677-f359fa10-0542-4967-a925-fac5dd26db6d.PNG)
	- In credentials, select the new key your added
	- Below, change Branches to build to `*/main`
<br> </br>
- In secion BUILD TRIGGERS, select `GitHub hook trigger for GITScm polling`
	- More on how to set up webhook below
- In section BUILD ENVIRONMENT select Provide Node & npm bin/ folder to PATH
- In section BUILD select `Execute Shell` and add 
```
cd app
npm install
npm test
```
- Save job
<br></br>
**CREATE NEW MERGE JOB**
- Follow same steps as before and add these new spets:
- In section SOURCE CODE MANAGEMENT.
- Change `Branch to build ` -> `*/dev`
- Select `Additional Behaviours` -> `Add` ->`Merge before build`

![Capture](https://user-images.githubusercontent.com/86292184/127513035-4e33c732-73ee-4738-9269-073a512fcae4.PNG)

- In section BUILD, select `Execute shell` and add 
```
git checkout main
git merge origin/dev
```
- In section POST-BUILD ACTIONS, 
![Capture](https://user-images.githubusercontent.com/86292184/127514664-57e708f3-bcfd-425b-8363-8c25a70bd979.PNG)
<br> </br>
- ------------------------------------
### Connect the new job to main
- Navigate to main job and open `Configure` and add:
![Capture](https://user-images.githubusercontent.com/86292184/127515968-62d26b0c-1bef-4427-8eed-7ae5876ecd03.PNG)
<br> </br>
- If all works well, once you build the main job, your connected builds should run also
- -------------------------------------
### github webhooks
- In github go to `settings` -> `webhooks`
- Add the jenkis ip + `/github-webhook/`
<br> </br>
- -----------------------------
### Create new github branch
- To create new branch:
-  on github `<> code`, find `main` and create new branch called `dev` 
- In termianl, run `git checkout -b dev`. This switches to new branch
- To push changes once commited, run `git push -u origin dev`
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
### Launch ami
- Select ami you would like to launch 
- Select launch
- Go though instance set up steps 
<br> </br>
- When ssh back into instance, make sure to change the ssh key from
`root` to `ubuntu`
- make sure ot update it `sudo apt-get update -y`
