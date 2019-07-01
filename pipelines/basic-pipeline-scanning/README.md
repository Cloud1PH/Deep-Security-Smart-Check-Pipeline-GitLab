# Basic Pipeline
## Description

This is a basic example of how Smart Check can be integrated into a GitLab pipeline.   

# Instructions
## Find & Replace

* **<gitlab_hostname>** - Gitlab hostname 
* **<registry_hostname>** - Docker registry hostname 
* **<registry_username>** - Docker registry username (default: testuser)
* **<registry_password>** - Docker registry password (default: testpassword) 
* **<dssc_hostname>** - Smart Check hostname
* **<dssc_username>** - Smart Check username
* **<dssc_password>** - Smart Check password

## Set up your project

1. Log into the GitLab UI and create a project:
	* **Name:** Basic Pipeline Scanning
	* **Visibility Level:** Private

	If you're asked to add an SSH, click **"Don't show again"** 
 
2. Set up git and clone the project:

	```
	git config --global user.name "Administrator"
	git config --global user.email "admin@example.com"
	git clone http://<gitlab_hostname>/root/basic-pipeline-scanning.git
	```

	When prompted, enter your GitLab username (`root`) and password.

3. 	Navigate to "Settings -> CI/CD". Make the following changes:
	1. Turn "Auto DevOps" off
	2. Configure the following variables:
		* **REGISTRY_HOSTNAME:** <registry_hostname>
		* **REGISTRY_USERNAME:** <registry_username>
		* **REGISTRY_PASSWORD:** <registry_password>
		* **DSSC_HOSTNAME:** <dssc_hostname>
		* **DSSC_USERNAME:** <dssc_username>
		* **DSSC_PASSWORD:** <dssc_password>
		
		**Note:** Mark all variables as "Protected" and click the "Hide values" button.
		

4. Navigate to the new directory and pull down the files required for this example:
	
	```
	cd basic-pipeline-scanning
	wget https://raw.githubusercontent.com/OzNetNerd/Smart-Check-Pipeline-GitLab/master/pipelines/basic-pipeline-scanning/files/Dockerfile
	wget https://raw.githubusercontent.com/OzNetNerd/Smart-Check-Pipeline-GitLab/master/pipelines/basic-pipeline scanning/files/index.html
	wget https://raw.githubusercontent.com/OzNetNerd/Smart-Check-Pipeline-GitLab/master/pipelines/basic-pipeline-scanning/files/banner.jpg
	https://raw.githubusercontent.com/OzNetNerd/Deep-Security-Smart-Check-Pipeline-GitLab/master/pipelines/basic-pipeline-scanning/files/.gitlab-ci.yml
	git add .
	git commit -m 'Initial commit'
	git push
	```

5. Browse to the CI/CD pipeline and watch it perform its tasks. 

6. Once the job fails, browse to the Smart Check UI and click "Scans" to examine the problem(s) Smart Check found. 