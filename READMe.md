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
- Create new build and add max # of builds to keep to 3
- Select `GitHub project` and add you github repos http
- section OFFICE 365 CONNECTOR select `Restrict where this project can be run` and add `sparta-ubuntu-node`
- section SOURCE CODE MANAGER select git.
	- In Reposiitory add repos ssh
	- Add new Credentials -> jenkins
	- Follow images set up and add PRIVATE key 
	![Capture](https://user-images.githubusercontent.com/86292184/127504677-f359fa10-0542-4967-a925-fac5dd26db6d.PNG)
	- In credentials, select the new key your added
	- Below, chane Branches to build to `*/main`
<br> </br>
- In secion 



- -----------------------------
### Create new github branch
- To create new branch:
-  on github `<> code`, find `main` and create new branch called `dev` 
- In termianl, run `git checkout -b dev`. This switches to new branch
- To push changes once commited, run `git push -u origin dev`
<br> </br>
- -------------------------------------
### github webhooks
- In github go to `settings` -> `webhooks`
- Add the jenkis ip + `/github-webhook/`
<br> </br>
- ------------------------

