![](https://wiki.jenkins-ci.org/download/attachments/2916393/logo-title.png "jenkins Logo Wiki")

# Sogeti Digital Bootcamp - Jenkins 2

In this bootcamp we will show you the basics on how to use Jenkins 2 as an automation tool. 
The bootcamp cosnsists of 5 steps for the basics and 1 advanced usage sample. 
Each of the 5 steps represent steps in your automation process and will either be an action to take (job) or automation of those tasks (pipeline). 
The advanced usage sample will teach you how to use DSL to parameterize your jobs and pipelines for easy re-use 

# Table of Contents
- [Installation](#jenkins-installation)
- [Configuration](#jenkins-configuration)
- [Step 1 - Create Checkout Job](#step-1---create-checkout-job)
- [Step 2 - Create Compile Job](#step-2---create-compile-job)
- [Step 3 - Create Unit Test Job](#step-3---create-unit-test-job)
- [Step 4 - Create Deployment Job](#step-4---create-deployment-job)
- [Step 5 - Create pipeline with all jobs](#step-5---create-pipeline-with-all-jobs)
- [Step 6 - Advanced Usage (DSL)](#step-6---advanced-usage-dsl)
- [additional tutorials on Jenkins](#-additional-tutorials-on-jenkins)

***

# Jenkins installation

[../Back to ToC](#table-of-contents)

For installation instruction including images (Windows) click [here](/docs/Install.md)

It would be silly to let you do a Jenkins Bootcamp without the software we are going to use mainly... Below are instructions to download Jenkins, additional software and to set up your GitHub Account.

In this Chapter we will discuss the following topics:

- Installation of Jenkins
- additional Software needed for the bootcamp
- set up a GitHub account and fork the bootcamp example

Note that we only discuss the windows versions of the software here

## Jenkins
Jenkins Can be downloaded from [Jenkins.io](http://www.Jenkins.io)

### install instructions

- Follow the install wizard
- [Configure Jenkins](#Jenkins-Configuration)

## additional software
Additionally for this bootcamp you will need Git and Java SDK. If you do not have these find the downloads below. 
Instructions with screens are added in the [Detailed instrcutions](/docs/Install.md)

- Git can be downloaded [here](https://git-scm.com/download/win)
- Java SDK can be downloaded [here](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

## github account
Since this bootcamp uses git it is mandatory to have a github account. If you already have an account you can skip creating one and Fork https://github.com/jeroenschepens/bootcamp-jenkins-example
- Go to [Github](http://www.github.com) and create a user account.
- go to [the bootcamps repo](https://github.com/jeroenschepens/bootcamp-jenkins-example)
- Fork the repository into your own account.
- clone the repo locally

if you're unsure how to use GitHub it is advised to do the [Hello World](https://guides.github.com/activities/hello-world/) course. Or [check out the video guides](https://www.youtube.com/githubguides)
the actions you need to focus on are
- Fork
- Clone
- Commit
- Push

or ask one of the instructors to help out

### Local repo instead of remote repo

instead of committing and pushing the changes into a forked reporsitory you can also choose just to clone the repo locally.

However instead of the repository url shown in the guide you will need to change the repository url to a local variant.

Windows:
```
file://D:\home\rbkcbeqc\dev\git\gitsandbox
```
Nix:
```
file:///home/rbkcbeqc/dev/git/gitsandbox
``` 

Ofcourse these need to be replaced by your actual cloned local repo.

***

# Jenkins Configuration

[../Back to ToC](#table-of-contents)

For configuration instruction including images (Windows) click [here](/docs/Config.md)

After Jenkins has been installed in will open default on "http://localhost:8080"

## Unlock Jenkins

Upon first run Jenkins will be locked and you will need to unlock it.

The gui will present you with a path and file you need to open.

In this file you will find a token string which needs to be copied into the Jenkins GUI.

Go with the suggested plugins and create a user

## additional plugins

In order to complete the bootcamp additional plugins are needed. Install the "Job DSL" plugin.
after that restart Jenkins

## Jenkins config

In global Tool configuration configure the following options

### Git

Name: Git
Path: __PathWhereYouInstalledGit__\git.exe (or find the path if you are using a nin-windows OS)

### Java

Name: Java SDK
JAVA_HOME: __PathWhereYouInstalledGit__ (don't include any libraries. Just the path is fine.)

### Maven

Name: M3
Check Install Automatically
Version 3.3.9


## Optional Configuration

### Change Port

If you don't want jenkins to run on 8080 or if another service you use runs on that port you can change it:

- find Jenkins.xml (in the install directory you choose in the wizard)
- search for "--httpPort=8080".
- replace 8080 with the desired port number
- restart jenkins (http://localhost:8080/safeRestart)
- alternatively you can start java on another port using "java -jar jenkins.war -httpPort=8081" (or any other port)

***

# Step 1 - Create Checkout Job

[../Back to ToC](#table-of-contents)

For detailed instruction including images click [here](/docs/Step1.md)

One of the first tasks in our automation process is to retrieve the code from version control to be used in the automation process. To do this we want to approach the Git Repository and checkout the code.

- Go to the Jenkins Dashboard (http://localhost:8080/)
- Go to "New item". Enter “Code Checkout” as the item name and choose "Freestyle project"
- Go to "Source code management" and select "Git". 
- Enter https://github.com/__username__/bootcamp-jenkins-example.git in the field "Repository URL".
- Make sure you use your own username
- Check that Branch specifies "*/Master"
- Click on "Save".

Note that if the repository URL is not entered correctly, Jenkins will show red text indicating it could not reach the URL along with an error description.

Now we want to test our job. so we start it manually.

- After saving click on the "Back to Dashboard" option to go back to the Jenkins Dashboard.
- Scroll down to find the created jobs.
- Click on "Schedule a build" icon all the way to the right to mark this job for execution
- Look at the icon on the left for the status. We see that it is Blue which indicates a successful execution.
- If you are interested in the details of the job execution you can click on the "#1" (or any other number) to open the specific job execution.

***

# Step 2 - Create Compile Job

[../Back to ToC](#table-of-contents)

For detailed instruction including images click [here](/docs/Step2.md)

Now that the code can be found on the git repository we can see if the code will compile / build and see if there are no syntax errors of any kind.

- Go back to your Jenkins Dashboard if not already there.
- Go to "New item". Enter “Compile Code” as the item name and choose "Freestyle project"
- Enter https://github.com/__username__/bootcamp-jenkins-example.git in the field "Repository URL".
- Make sure you use your own username
- Go to "Build" and select "ADD BUILD STEP". Choose "Invoke top-level Maven targets". Select "M3" in the "Maven Version" dropdown menu.
- Enter "clean compile" in the "Goals" field.
- Click on "Save".

Again we have to run this job. We can go back to the Jenkins Dashboard and schedule it immediately or we can click the "Build Now" Option after saving the previous steps.
Please have a look at the results.. and see if anything went wrong.


We can see that instead of the blue icon we have a red icon. Something went wrong in executing this job. Open the details to find out why the job did not succeed. Click "Console Output for actual build info"
Note: The first time running this job may need addition libraries for maven. This can clog the console output with download info. 
Actual output can be found on the bottom. Alternatively you can re-run the build and see the Console output directly.

Can you find out what went wrong with the code and why it did not build? Go and edit the code and re-run the job until it completes.

Alternatively you can change the branch in the job in the "Source Code management" tab to "*/step2".
Re-run the job and take a look at the outcome.

Note on the jenkins Dashboard & the Weather Icon: 

The Weather icon indicates how the past couple of runs have been executed:

Sunny weather means nothing went wrong. 
Cloudy weather means some executions were unsuccessful. 
Thunderstorms means most of the past runs were unsuccessful.

***

# Step 3 - Create Unit Test Job

[../Back to ToC](#table-of-contents)

For detailed instruction including images click [here](/docs/Step3.md)

Now that our code compiles we would like to run some tests. For Maven we can request to perform a standard set of Unit tests. Other languages might have a different implementation

- Go back to your Jenkins Dashboard if not already there.
- Go to "New item". Enter “Test” as the item name and choose "Freestyle project"
- Enter https://github.com/__username__/bootcamp-jenkins-example.git in the field "Repository URL".
- Make sure you use your own username
- Go to "Build" and select "ADD BUILD STEP". Choose "Invoke top-level Maven targets". Select "M3" in the "Maven Version" dropdown menu.
- Enter "clean test" in the "Goals" field.
- Click on "Save".

Now we are going to execute the unit test. Click "Build Now" to start the job.

Take a look at the outcome and the console output for the job. Try and fix the error.

Alternatively you can change the branch in the job in the "Source Code management" tab to "*/step3".
Re-run the job and take a look at the outcome.

***

# Step 4 - Create Deployment Job

[../Back to ToC](#table-of-contents)

For detailed instruction including images click [here](/docs/Step4.md)

Since we can't actually deploy our test application we have chosen to simulate this step by creating a notification step instead.

- Go back to your Jenkins Dashboard if not already there.
- Go to "New item". Enter “Deploy” as the item name and choose "Freestyle project"
- Enter https://github.com/__username__/bootcamp-jenkins-example.git in the field "Repository URL".
- Make sure you use your own username
- Go to "Build" and select "ADD BUILD STEP". Choose "Invoke top-level Maven targets". Select "M3" in the "Maven Version" dropdown menu.
- Enter "clean build" in the "Goals" field.
- go to the "Post-build actions" tab and define an email notification
- enter your own mail address or "bootcamp2017-jenkins@mailinator.com" into the mail address
- additionally you can check either boxes to have individual mails sent (not configured) or to send a mail upon broken builds 
- Click on "Save".

- Run the project and take a look at your mail address or https://www.mailinator.com/inbox2.jsp?public_to=bootcamp2017-jenkins to see if the step succeeded. 

Probably not because we did not configure our e-mail settings in Jenkins.

- Go back to your Jenkins Dashboard if not already there.
- Go to the Jenkins configuration 
- configure the email settings. A good source for this is the gmail SMTP
- After configuring you can let Jenkins send a test mail. Please do so to test the configuration


The following settings can be used

```
SMTP Server: SMTP.gmail.com
Use SMTP Auth: Yes
Username: full gmail address
password: ************
Use SSL: Yes
Port: 465
```

[Two-factor auth](http://stackoverflow.com/questions/26736062/sending-email-fails-when-two-factor-authentication-is-on-for-gmail) requires a different approach.

Try the job several times working or not working and behold the notifications (spam) coming in.

***

# Step 5 - Create pipeline with all jobs

[../Back to ToC](#table-of-contents)

For detailed instruction including images click [here](/docs/Step5.md)

In this step we are introducing two topics:

- Build (job) Triggers
- Chaining Jobs / Projects in a pipeline.

Here we get to the nitty gritty of things. In the past 4 steps we created the separate jobs we use in our Continuous Integration cycle. There's one caveat thought. As a developer or ops you don't have time to start each job whenever you process a commit on your code.

As Jenkins is an Automation tool after all we would like to proces all these job at once, preferably when new code is committed to the repository or in Daily / Nightly builds.

## Build Triggers (or Scheduling)

Build Triggers are important in automation because they initiate the first step in kicking off a chain of jobs or a pipeline.

In jenkins there are three types of triggers:

- Build Trigger -> available in a Job / pipeline to specify when the build step should start
- Post Build Actions -> Avaliable in a Job only and specifies a follow-up action after build has taken place (see Step4).

# kan iemand dit controleren???

In our bootcamp we will build two triggers:

- Upstream Trigger on Git to start the "Checkout Code" Job
- Upstream Trigger on our pipeline (built later) to watch when "Checkout Code" has finished

__note on terminology: Upstream is when a project / Pipeline executes a separate Project / Pipeline as part of its execution whereas a Downstream trigger is the Executed Project / Pipeline from another Project / Pipeline.__

#graag even nakijken of deze klopt!!

### Upstream Trigger on Git to start the "Checkout Code" Job

In order for the trigger to work we need to reconfigure our "Code Checkout" Job

- Open the "Code Checkout" Job
- Go to the tab "Build triggers"
- Select "Poll SCM" and Enter "* * * * *" as Value. This will poll SCM every minute. If you want a different value use another [CRON Expression](https://en.wikipedia.org/wiki/Cron)
- Click "Save"

A better way to trigger this job is to listen to Git Commits done when Git pushes the Git Command. This Requires the Git Plugin version 1.1.14 and an alteration to your local git. [Use this guide](http://kohsuke.org/2011/12/01/polling-must-die-triggering-jenkins-builds-from-a-git-hook/) to set it up. Use at your own risk!

Try your new trigger by pushing code to the remote repository and see if the "Code Checkout" job starts.

### Upstream Trigger on our pipeline (built later) to watch when "Checkout Code" has finished

We will configure this during the construction of our Pipeline.

## Pipeline

Constructing Pipelines in jenkins is done in the Groovy language and stored in a __Jenkinsfile__ which is to be stored in the code repository so it can be used upon code checkout.

A jenkinsfile will follow a set of instructions to configure Stages, Steps, Parameters etc.

The jenkinsfile starts with a __pipeline{}__ command along with the specification of a specific or any agent.

```
pipeline {
    agent any

}
``` 
Within this syntax follow various __stages{}__ , __stage('name'){}__  and __steps{}__ commands to specify what needs to be done in the pipeline.

We are going to create our first "Hello Pipeline" Pipeline.

- Go back to your Jenkins Dashboard if not already there.
- Go to "New item". Enter “Manual Pipeline” as the item name and choose "Pipeline"
- in The Build Triggers tab check "Build after other projects are built" and choose "Code Checkout"
- in the Pipeline tab under Definition select "Pipeline Script"
- write code to create a pipeline with the 3 remaining stages (steps) with each one step using the "build job:'name'" command.
- click "Save"

need help creating pipeline syntax?

either click on pipeline syntax in the configure screen or check this link: http://localhost:8080/job/Manual%20Pipeline/pipeline-syntax/

Push a change to your Remote repo to fire off the "Code Checkout" job which will start the pipeline for the remaining

go to the pipeline and select "Stage view" to get a visual representation of the view.

to further automate the building process specific to the code you want to automate we may want to run different jobs or in a different order.

- The code you have written could be saved in a __"Jenkinsfile"__ and stored along with the rest of the code. an Empty example can be found in the Sourcecode folder.
- Go back to your Jenkins Dashboard if not already there.
- Go to "New item". Enter "Jenkinsfile Pipeline” as the item name and choose "Pipeline"
- to use this script in the pipeline select "pipeline script from SCM" in the pipeline
- add the git remote repository
- Select the appropriate branch
- In path leave the setting to Jenkinsfile to search the root of the repository

If you are having troubles getting your Jenkinsfile working you can point the SCM to "*/step5"

Have a look at that super shiny awesome looking pipeline you just created. yes.... you may pat yourself on the back ^^

***

# Step 6 - Advanced Usage (DSL)

[../Back to ToC](#table-of-contents)

For detailed instruction including images click [here](/docs/Step6.md)

No Content Yet.

more info on using Job DSL can be found [here](https://jenkinsci.github.io/job-dsl-plugin/)

***

# Additional tutorials on Jenkins

[../Back to ToC](#table-of-contents)

- [Game of Life](https://github.com/tsteenbakkers/game-of-life) (Maven)
    - [O'reilly - John Ferguson Smart - Jenkins: The definitive Guide](/docs/jenkins-the-definitive-guide.pdf)
- [Job DSL](https://jenkinsci.github.io/job-dsl-plugin/)

***

 [Back to top](#table-of-contents)