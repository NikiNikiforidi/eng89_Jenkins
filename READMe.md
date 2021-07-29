# Jenkins


![jenkins](https://user-images.githubusercontent.com/86292184/127477136-859476bd-3f40-4d39-8a69-c4c9fd7501f7.png)


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
## Jenkins CIDC