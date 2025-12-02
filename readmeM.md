
I.	Steps for MavenJava Automation:
Maven Java Automation Steps:
 Step 1: Open Jenkins (localhost:8080)
   	 ├── Click on "New Item" (left side menu
Step 2: Create Freestyle Project (e.g., MavenJava_Build)
        	├── Enter project name (e.g., MavenJava_Build)
        	├── Click "OK"
└── Configure the project:
            		├── Description: "Java Build demo"
            		├── Source Code Management:
            			└── Git repository URL: [GitMavenJava repo URL]
            		├── Branches to build: */Main   or  */master
  		└── Build Steps:
               	     ├── Add Build Step -> "Invoke top-level Maven targets"
                  		└── Maven version: MAVEN_HOME
                 		└── Goals: clean
                	├── Add Build Step -> "Invoke top-level Maven targets"
                		└── Maven version: MAVEN_HOME
                		└── Goals: install
                	└── Post-build Actions:
                    	       ├── Add Post Build Action -> "Archive the artifacts"
                    			└── Files to archive: **/*
                    	     ├── Add Post Build Action -> "Build other projects"
                    			└── Projects to build: MavenJava_Test
                    			└── Trigger: Only if build is stable
                    	     └── Apply and Save


    └── Step 3: Create Freestyle Project (e.g., MavenJava_Test)
        	├── Enter project name (e.g., MavenJava_Test)
        	├── Click "OK"
              └── Configure the project:
             ├── Description: "Test demo"
             ├── Build Environment:
            		└── Check: "Delete the workspace before build starts"
            ├── Add Build Step -> "Copy artifacts from another project"
            		└── Project name: MavenJava_Build
            		└── Build: Stable build only  // tick at this
            		└── Artifacts to copy: **/*
            ├── Add Build Step -> "Invoke top-level Maven targets"
            		└── Maven version: MAVEN_HOME
            		└── Goals: test
            		└── Post-build Actions:
                ├── Add Post Build Action -> "Archive the artifacts"
                	└── Files to archive: **/*
                	└── Apply and Save

    └── Step 4: Create Pipeline View for Maven Java project
        ├── Click "+" beside "All" on the dashboard
        ├── Enter name: MavenJava_Pipeline
        ├── Select "Build pipeline view"         // tick here
         |--- create
        └── Pipeline Flow:
            ├── Layout: Based on upstream/downstream relationship
            ├── Initial job: MavenJava_Build
            └── Apply and Save OK

    └── Step 5: Run the Pipeline and Check Output
        ├── Click on the trigger to run the pipeline
        ├── click on the small black box to open the console to check if the build is success
            Note : 
1.	If build is success and the test project is also automatically triggered with name       
                      “MavenJava_Test”
2.	The pipeline is successful if it is in green color as shown ->check the console of the test project
3.	The test project is successful and all the artifacts are archived successfully
*********************************
II. Maven Web Automation Steps:
└── Step 1: Open Jenkins (localhost:8080)
    ├── Click on "New Item" (left side menu)
    
    └── Step 2: Create Freestyle Project (e.g., MavenWeb_Build)
        ├── Enter project name (e.g., MavenWeb_Build)
        ├── Click "OK"
        └── Configure the project:
            ├── Description: "Web Build demo"
            ├── Source Code Management:
            		└── Git repository URL: [GitMavenWeb repo URL]
            ├── Branches to build: */Main or master
            └── Build Steps:
                ├── Add Build Step -> "Invoke top-level Maven targets"
                	└── Maven version: MAVEN_HOME
                	 └── Goals: clean
                ├── Add Build Step -> "Invoke top-level Maven targets"
                	└── Maven version: MAVEN_HOME
                	└── Goals: install
                └── Post-build Actions:
                    ├── Add Post Build Action -> "Archive the artifacts"
                   	 └── Files to archive: **/*
                    ├── Add Post Build Action -> "Build other projects"
                    	└── Projects to build: MavenWeb_Test
                    	└── Trigger: Only if build is stable
                    └── Apply and Save

    └── Step 3: Create Freestyle Project (e.g., MavenWeb_Test)
        ├── Enter project name (e.g., MavenWeb_Test)
        ├── Click "OK"
        └── Configure the project:
            ├── Description: "Test demo"
            ├── Build Environment:
            		└── Check: "Delete the workspace before build starts"
            ├── Add Build Step -> "Copy artifacts from another project"
            		└── Project name: MavenWeb_Build
            		└── Build: Stable build only
            		└── Artifacts to copy: **/*
            ├── Add Build Step -> "Invoke top-level Maven targets"
           		└── Maven version: MAVEN_HOME
            		└── Goals: test
            └── Post-build Actions:
                ├── Add Post Build Action -> "Archive the artifacts"
                	└── Files to archive: **/*
                ├── Add Post Build Action -> "Build other projects"
                	└── Projects to build: MavenWeb_Deploy
                └── Apply and Save

    └── Step 4: Create Freestyle Project (e.g., MavenWeb_Deploy)
        ├── Enter project name (e.g., MavenWeb_Deploy)
        ├── Click "OK"
        └── Configure the project:
            ├── Description: "Web Code Deployment"
            ├── Build Environment:
            		└── Check: "Delete the workspace before build starts"
            ├── Add Build Step -> "Copy artifacts from another project"
            		└── Project name: MavenWeb_Test
            		└── Build: Stable build only
               	└── Artifacts to copy: **/*
            └── Post-build Actions:
                ├── Add Post Build Action -> "Deploy WAR/EAR to a container"
   └── WAR/EAR File: **/*.war
   └── Context path: Webpath
 └── Add container -> Tomcat 9.x remote
└── Credentials: Username: admin, Password: 1234
── Tomcat URL: https://localhost:8085/
                └── Apply and Save

    └── Step 5: Create Pipeline View for MavenWeb
        ├── Click "+" beside "All" on the dashboard
        ├── Enter name: MavenWeb_Pipeline
        ├── Select "Build pipeline view"
        └── Pipeline Flow:
            ├── Layout: Based on upstream/downstream relationship
            ├── Initial job: MavenWeb_Build
            └── Apply and Save

    └── Step 6: Run the Pipeline and Check Output
        ├── Click on the trigger “RUN” to run the pipeline
            Note: 
1.	After Click on Run -> click on the small black box to open the console to check if the build is success
2.	Now we see all the build has  success if it appears in green color
        ├── Open Tomcat homepage in another tab
        ├── Click on the "/webpath" option under the manager app
               Note:
1.	 It ask for user credentials for login ,provide the credentials of tomcat.
2.	It provide the page with out project name which is highlighted.
3.	After clicking on our project we can see output.

**********************
in chrome type ==> aws student login ==> login with ur credentials
1. click on courses ==> click on modules
2. Scroll down and select Launch AWS Academy Lab
3. click on start lab( red turns to green)
4. then click on that aws green dot
5. click on ec2 instance
6. Click on Launch Instance
7. give name (e.g: MyWebServer), select option ubuntu ==> Instance type t2.micro
8. Create a new keypair---> Give KeyPair name and click on create key pair ==> a keypair will downloaded  with extension .pem ==> Store key      in folder AWS which shud be created on desktop
9. click all check boxes(http and https)
10. Storage - 8GB
11. click on launch instance
12. Now click on the instances ==> Wait for the Success message to apper in green color
13. You have to get 2 tests passes. 
Important:---- Now check the box and click on connect
14.Now you can connect local system to server (EC2 instance) using secure shell SSH ==> go to shh client, copy the ssh command.
15. Copy the path of .pem file that you saved earlier in folder AWS 
16. Open Powershell in administrative mode and navigate to that path. 
     Type:   cd < path> 
17. Go to SSh and copy the command ssh and paste it in powershell==>type yes
18. * sudo apt update
    * sudo apt-get install docker.io
    * sudo apt install git
    * sudo apt install nano
19. Create another folder on desktop and Create basic index.html file in folder Example and save it
20. Open git Bash in folder Example by right clicking with mouse
    * git init
    * git add .
    * git commit –m “first commit”
21. Create git repository (here with name AWS)
   * git branch –M main
   * git remote add origin <https url>
   * git push –u origin main
22. Copy http path of the repo
23 In the powershell inside ubuntu type this ==>git clone <copied http url>
   * cd AWS
   * ls 
   * nano Dockerfile
 --> paste these (for index.html) ==> 
   FROM nginx:alpine
   COPY . /usr/share/nginx/html
 --> (for mavenweb project paste) ==> 
  FROM tomcat:9-jdk11
  COPY target/*.war /usr/local/tomcat/webapps 
==>ctrl+s and ctrl+x
24. Build docker image by executing the following command
   * sudo docker build –t mywebapp .
25 run the image
 * sudo docker run –d –p 80:80 mywebapp
26. copy public ipv4 address
:  Paste it in the browser to get below output
Note : if https :__ won’t work then type just http:___
27. Run the following command to stop the container
 * sudo docker ps
 * sudo docker stop <container-id>
28.Go back to instances and click on option instance. Once load as below, then Click on Instance state and select terminate instances
29. Click on Terminated (deleted)==> Now click on End lab

for web after 18 direct go to 23






