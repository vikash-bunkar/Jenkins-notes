# Jenkins-notes
Edureka Notes - Sonal Mittal

Jenkins Set up:

Create an ec2 Instance or use the same instance used in GIT sessions.

If git is not present:

Git installation:

yum install git -y

Check the version of GIT

…
git --version
…

SEE THAT YOU ARE IN ROOT DIRECTORY
# cd
# pwd
************************************

JAVA Installation

SEE THAT YOU ARE IN ROOT DIRECTORY

…
 sudo amazon-linux-extras install java-openjdk11
…

IF Command to install java on CENTOS 8

..
sudo yum install java-11-openjdk-devel
…

Command to install java 11 on Ubuntu

…
apt-get update 
apt-get install openjdk-8-jdk

…


On the browser open https://www.jenkins.io/

Go to downloads section

Select centos OS

Go to Ec2 machine and execute commands:

 sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
  sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
   yum install jenkins -y

Execute command to start jenkins server:

systemctl start jenkins


systemctl status jenkins

Installation is complete on the server

We need to now  set up jenkins dashboard

For this, take public ip address of ec2 server, copy it and go to your browser and give

publicipaddress:8080

On the ec2 server, execute below command

$ cat /var/lib/jenkins/secrets/initialAdminPassword

Copy the password and paste in the browser (jenkins)

Click on continue

Click on Jenkins suggested plugin tab(on left side)

On the next page

Username: admin
Password : admin
Retype password: admin
Email: admin@gmail.com

Click on continue

Click on finish

You will be on the jenkins dashboard

****************************************************
 Create a Jenkisn job that will clone a github repository in jenkins workspace

Create a new job in jenkins
Click on + sign to create new item/job/project
Give a name to the job : CloneRepo
Select freestyle project and click on OK button
On the project click on Source code management
Select git option
Give git hub repo path
https://github.com/Sonal0409/myproject05Aug

Branch name as ===>  Master

Save the job

Click on Build now

Repository will be cloned in jenkins workspace

You can go to job → workspace folder to see the files

****************
Day 2:

Demo 1: Create a Jenkisn job that will clone a github repository in jenkins workspace

Create a new job in jenkins
Click on + sign to create new item/job/project
Give a name to the job : CloneRepo
Select freestyle project and click on OK button
On the project click on Source code management
Select git option
Give git hub repo path
https://github.com/Sonal0409/myproject06nov.git

Branch name as ===>  Master

Save the job

Click on Build now

Repository will be cloned in jenkins workspace

You can go to job → workspace folder to see the files

*****************************
Create a new job → general section→ click on advance → custom workspace
Give directory as /tmp/myworkspace
Save job → build
*********************************

Wipe out the workspace

On the job → go to build environments → Select first checkbox →Delete workspace before build

****************************************

Jenkins Triggers.
     To automatically execute a jenkins job, you will use Jenkins triggers
       Based on:

      Periodic time- schedule
      Whenever there is a commit in github repo
      A parent job can trigger execution of child job
      Execute a job from remote script

You will go to build triggered section in the job to set up a trigger for automatic execution of the job


Trigger prieodically









Go to Job Clone repo --> build trigger--> build preodically --> give */2 * * * * --> Save and build now


All build will be generated automatically every 2 mins


Build Triggers--> Poll SCM --> * * * * *









Build will be generated each time there is a chnage in the repository only.



Webhook:
Webhooks allow Jenkins to be notified when certain events happen on the repo. When the specified events happen, we’ll send a POST request to each of the URLs you provide.



GitHub hook trigger for GITScm polling

> Select the above option

Save the job and now go to git hub

Always save the job and then only  go to git hub, perform these steps

go to the repository setting
select webhooks on left side
delete if any exisitng webhook

click on createwebhook on right side

provide following information:

Payload URL : jenkinsurl/github-webhook/ ===> http://3.140.252.165:8080/github-webhook/

Content type : select application/json

Secret: no need of any value

Which events would you like to trigger this webhook?

select first option -- Just the push event.

Select Active
click on add webhook.

now make some changes in repo,
go to jenkins--> you will see a new build has been created



Day 3: Build pipeline using Jenkins, Maven & GIT
***************************

Build too : Maven
***************************
> Powerful project management tool, This tool will help a developer to write his source code and test scripts and maintain them locally

> maven at heart is a plugin based tool (plugin execution framework)
> It is used for Java based code
> All the work in maven is done by plugins (extensions, which when downloaded and executed will enhance the features of actual tool)
> Compile the code → Maven plugin
> 2 types of plugin in Maven:

    > Core Build plugins
  
Tasks				Plugin				Command (GOAL)

Compile the code		Compiler			compile
Execute Test cases		Surefire 			test
Package the code		package			package
Delete previous 
build files			Clean				clean
Check dependencies		install				install
Deploy the package to
Artifactory			deploy			deploy
   > Reporting Plugins
Plugins that are needed for validation and review the code and generate reports
Reports by maven plugins are generated in the format : .xml, .csv, .html or .txt

Tasks 				Plugin				Command(GOAL)

Code review
(Static Analysis tool)		pmd				pmd:pmd

Test reports			surefire-reporting		test

Code Coverage		cobertura(deprecated)
				JACOCO
Code review
 & Code Coverage                  Sonarqube			sonar:sonar


Code Review with PMD: The PMD Plugin allows you to automatically run the PMD code analysis tool on your project's source code and generate a site report with its results.

Code Coverage : Using code coverage  tool against your compiled classes it helps you determine how well the unit testing and integration testing efforts have been, and it can then be used to identify which parts of your Java program are lacking test coverage.

A developer will create a Maven project locally in his/her IDE and write source code and test cases inside it

Traditionally, they will build all the above steps in their local machine.

Every project will include following folders:

/src/main/java  ⇒ main java source code

/src/test/java  ⇒ test cases will be present

In a maven project, you will have the main POM.xml
Which created by Java developer
This file will be prepared before you write the code
This file will be updated while the code

In this file Java developer will write : 

 > name of the artifact which will be build
> type of artifact
> Dependencies ⇒ softwares that will download automatically for code to work
****
Build section:
> name and version of plugins
    Which all plugin we will use for build process


SetUp
**************************

Jenkins--> Manage Jenkins-->Global Tool configuration

Under GIT==> leave it same 

Under maven ==> let's install it automatically
Type name as  mymaven 
check the install automatically box.
Save the changes


****************************************

CREATION OF COMPILE JOB
***********************************

1. new Job --1.complie-->freestyle project
2. Source code management --> select git
--->give git path ===> https://github.com/Sonal0409/DevOpsCodeDemo.git

3. build ==> select invoke top level maven targets
   select maven version===> mymaven
    goal ==> compile
4. save ==> build now 
So all the compiled files will be present at this location
==> look for this line in console output at the end on jenkins
Compiling 13 source files to /var/lib/jenkins/workspace/compile1/target/classes

5. Go to workspace on the left side of jenkins Job compile
In the folder go to ==> target folder==> go to classes/com/edurekademo
==> click on edurekademo==> got to utilities==> all class files will be there which have recently been compiled.
******************************
2nd JOB ==>Code Review
*********************

Jenkins--> new item--> Name= code Review==> freestyle project
==> source codemanagement==>select git==> 
give git repo name https://github.com/Sonal0409/DevOpsClassCodes

Step 2: build ==> invoke top level maven target==>
 select maven version==>mymaven ==> goal = pmd:pmd

save==> build now
==> click on build number and see the console

Goto Workspace on the left side of jenkins Job code review
In the folder go to ==> target folder ==> you will find pmd.xml file.

CONVERT FILE TO TREND REPORT
*******************************
 manage jenkins--> manage plugins--> available --> Search for warning next gen plugin--> install it.
Retsrat in case plugin fails installation.

Now go to the job Code Review==> go to post build actions ==> select recordcomplier warnings and static result analysis

Under tool ==> select PMD
under report file format ==> give path of pmd.xml file ie:  target/pmd.xml
or as mentioned in message below : copy **/pmd.xml  ( no quotes)

Save the file and build now.

After the build is successfull, you will see PMD Warnings

So you will see 12 new warning have been generated on the code.
Scroll down 
Under package click on first one to check warnings.

*********************************
JOB3: Testing --Unit Test report
********************************

1. jenkins--> new item--> Name= 3.unitTest==> freestyle project
==> source codemanagement==>select git==> give git repo name 
https://github.com/Sonal0409/DevOpsCodeDemo.git

2. build ==> invoke top level maven target==> select maven version==>mymaven ==> goal = test

3. Save and Build now.

4. Check the workspace

5. surefire-reports will be present

But we cannot understand them easily. So lets generate understandable reports by using Junit reports option under post build actions

6. Go back to job==> post build actions==> select junit test result job
==> give target/surefire-reports/*.xml  ===>under test report xml
This is path where xml files are there.
Here *.xml is as we want to use all the xml files

7. Save and build again

Go to build number ===> on left side you will see Test Result 
Click on it and you can see all pass and fail details of the tests


**********************************
Job4  Package Job
***************************

Go to jenkins--> new item--> Name= Package==> freestyle project
==> source codemanagement==>select git==> give git repo name https://github.com/Sonal0409/DevOpsClassCodes
 
build ==> invoke top level maven target==>mymaven

 goal = package

==> save==> build now
==> click on build number and see the console

==> go to workspace ==> target/addressbook.war


**************
Connecting all the 4 jobs so that each job can be triggered:


Go to manage plugins → available section

Search for build pipeline plugin

Click on the checkbox and click on Install without restart


Plugin will be installed. It will display all connected jobs on the dashboard

This plugin falls in the category of Jenkins view. It creates a build pipeline view

Lets build the pipeline:

********************************
*****************

Day4:
********************

******************************
Pipeline as a code
**************************
Lets create a pipeline using code:


Create a new job ==> select project as pipeline


pipeline{
    
    tools{
        // what tool version to use for build stages
        maven 'mymaven'
    }
    
    agent any
    
    stages{
        
        stage ('CloneRepo')
        {
            steps{
             echo 'This is stage 1'
              git 'https://github.com/Sonal0409/DevOpsCodeDemo.git'  
            }
        }
        
        stage ('Compile')
        {
            steps{
                
             sh  'mvn compile'
                
            }
        }
        stage ('CodeReview')
        {
            steps{
                
             sh  'mvn pmd:pmd'
                
            }
            post{
                success{
                    recordIssues(tools: [pmdParser(pattern: '**/pmd.xml')])
                }
            }
        }
        stage ('Test')
        {
            steps{
                
             sh  'mvn test'
                
            }
        post{
            success{
                junit 'target/surefire-reports/*.xml'
            }
        }
        }
        
        stage('package')
        {
            steps{
                sh 'mvn package'
            }
        }
        
    }
    
    
    
}


Day 5: Agenda:

> Jenkinsfile

It is a simple text file with no extension
This file contains declarative pipeline code for CI/CD pipeline
The name of this file is always Jenkinsfile
This will is maintained in the root directory of your project repo on SCM tool
Every team member can now make changes to the pipeline code without logging on the jenkins server.
In the github we can also maintain every version of pipeline code
We write the pipeline code in IDE and push the Jenkinsfile in the repository
In jenkins you will create a job, that will fetch the Jenkinsfile and jenkins will execute the pipeline for you.

Demo:

Click on new Item → give the job name as JenkinsfileDemo → Select pipeline project→ press OK
In the job template → click on pipeline tab → select the dropdown option as Pipeline script from SCM
Select SCM as → git
Repository URL? ⇒ https://github.com/Sonal0409/DevOpsCodeDemo.git

We will also add a trigger → Go to Build triggers tab above→ POLL SCM → * * * * *

So that if there is any commit, my pipeline should get executed everytime.


> MultiBranch Pipeline

Basically we have many branches in github for a repository
Each branch may have separate set of tasks/jobs to be executed
Whenever a branch is created on the repo, Make sure the branch has a Jenkinsfile init
In the Jenkinsfile you write the code which is needed to execute certain tasks related to the branch
we will set up a job in jenkins that will scan every branch of your github repository
Whichever branch has the jenkinsfile present on it
            > for that branch a jenkins pipeline job will be created with same name as branch name  and Jenkinsfile code will be executed

Whichever branch of your repo does not have Jenkinsfile on it. No job will be created for it.


Demo:

Click on new item→ create job -> give name as MultibranchdemoJob → select the template as Multibranch Pipeline → press Ok

In General section → give Display name as :  Multibranch Pipeline

In branch Sources section → Click on Add source → select git

Give git  Project Repository -> https://github.com/Sonal0409/MultiBranchDemo.git

Nomore changes and save the job.

Jenkins will automatically scan the branches of the repo and create pipeline jobs.

Click on multibranch pipeline job name and you will see the 3 jobs created for 3 branches.



> DSL scripts for automation:

Jenkins DSL is a programmatic way to implement the jenkins JOB
In order to write DSL script, you will have to use DSL plugin which will automatically create jobs in jenkins for you
Using the DSL plugin you will be able to create jenkins job on the fly just with help of simple groovy code.
Groovy scripts are very easy to write and read, it is like java

Demo:

Install DSL plugin

Go to Manage Jenkins → Manage plugins → Available → search for DSL → select the plugin Job DSL plugin → click on install without restart.
Now Go to Manage jenkins → Configure global security →Scroll down to CSRF protection and
Uncheck  → Enable script security for Job DSL scripts


Important:
**************
We have to create a job with name as seedjob → in this job we will write the DSL scripts
The operation of seed job is to create new jobs programmatically
Seed job will always be a Freestyle job
In the seed job under Build steps you will write the DSL scripts and execute the seed job.

*******************
DSL script1:
***************
Write DSL script to automatically create a job with name as Firstjob and description as “my first DSL job”

Demo: 
Create a Seed Job in jenkins
New item→ SeedJob → freestyle project–press OK
Go to Build steps → Process Job DSLs —> 
Select the option -> Use the provided DSL script
Define variable in groovy and use it in Job

def jobname = 'Firstjob'

job(jobname){
}


*******
Create job with Description

def jobname = 'Firstjob'

job(jobname){

 description('my testjob')
triggers{
 cron ('* * * * *')
}
}


**************

ADD SCM option

def jobname = 'Firstjob'
def repo = 'sonal0409/DevOpsClassCodes'

job(jobname){

 description('my testjob')

scm {
        github repo
    }
}

****************
Add triggers & build step

def jobname = 'Firstjob'
def repo = 'sonal0409/DevOpsClassCodes'

job(jobname){

 description('my testjob')

scm {
        github repo
    }
triggers{
 cron ('H/15 * * * *')
}

steps {
 shell ("echo 'Hello World'")
}
}

*********************

Delete job created programitically

jenkins.model.Jenkins.theInstance.getProjects().each { job ->
    if (job.name.contains('job')) {
        println job.name
        job.delete()
    }
}

OR

jenkins.model.Jenkins.theInstance.getProjects().each { job ->
    if (job.name.contains('Job')) {
        println job.name
        job.delete()
    }







Upstream/Downstream pipeline using DSL script job

job('job-checkout') {
    
    scm {
        github('sonal0409/DevOpsClassCodes', 'master')
    }
      
   publishers {
        downstream 'job-compile', 'SUCCESS'
    }
    
}
job('job-compile'){
   
  scm {
        github('sonal0409/DevOpsClassCodes', 'master')
    }

  steps{
  maven('compile')
  }
 publishers {
        downstream 'job-test', 'SUCCESS'
   }
}

job('job-test'){
   
  scm {
        github('sonal0409/DevOpsClassCodes', 'master')
    }
  steps{
   maven('test')
  }
 publishers {
        downstream 'job-package', 'SUCCESS'
   }
}

job('job-package'){
   
   scm {
        github('sonal0409/DevOpsClassCodes', 'master')
    }
  steps{
  maven('package')
  }
}

********************************************
