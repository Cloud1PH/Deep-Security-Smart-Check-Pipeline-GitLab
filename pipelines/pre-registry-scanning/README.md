# Dual Registry Pipeline (Pre-Registry Scanning)
## Description

This example replicates a typical DevOps pipeline. Upon modification, Docker images are sent to a "dirty" registry which is **only available to developers and Smart Check**.  Smart Check will examine the image's integrity and if an issue is found, it will fail the pipeline. 

If no issues are found, the pipeline progresses and the image is placed into a "blessed" registry. This registry is available to users for consumption.

This two staged approach ensures that only known safe images are available for consumption.        

# Instructions
## Find & Replace

* **<blessed_registry_hostname>** - Blessed Docker registry hostname 
* **<blessed_registry_username>** - Blessed Docker registry username (default: testuser)
* **<blessed_registry_password>** - Blessed Docker registry password (default: testpassword)
* **<pre_registry_username>** - Smart Check built-in registry username (default: testuser)
* **<pre_registry_password>** - Smart Check built-in registry password (default: testpassword)
* **<dssc_hostname>** - Smart Check hostname
* **<dssc_username>** - Smart Check username
* **<dssc_password>** - Smart Check password

## Set up your project

1. Log into the GitLab UI and create a project:
	* **Name:** Dual Registry Pipeline
	* **Visibility Level:** Private

	If you're asked to add an SSH, click **"Don't show again"** 
 
2. 	Navigate to "Settings -> CI/CD". Make the following changes:
	1. Turn "Auto DevOps" off
	2. Configure the following variables:
		* **BLESSED_REGISTRY_HOSTNAME:** <blessed_registry_hostname>
		* **BLESSED_REGISTRY_USERNAME:** <blessed_registry_username>
		* **BLESSED_REGISTRY_PASSWORD:** <blessed_registry_password>	
		* **DSSC_HOSTNAME:** <dssc_hostname>
		* **DSSC_USERNAME:** <dssc_username>
		* **DSSC_PASSWORD:** <dssc_password>
		* **PRE_REGISTRY_USERNAME:** <pre_registry_username>
		* **PRE_REGISTRY_PASSWORD:** <pre_registry_password>
		
		Click "Save variables"

3. Set up git and clone the project:

	```
	git config --global user.name "Administrator"
	git config --global user.email "admin@example.com"
	git clone http://<gitlab_hostname>/root/pre-registry-scanning.git
	```
	
	When prompted, enter your GitLab username (`root`) and password.
		
4. Navigate to the new directory and pull down the files required for this example:
	
	```
	cd pre-registry-scanning
	wget https://raw.githubusercontent.com/OzNetNerd/Deep-Security-Smart-Check-Pipeline-GitLab/master/pipelines/pre-registry-scanning/files/Dockerfile
	wget https://raw.githubusercontent.com/OzNetNerd/Deep-Security-Smart-Check-Pipeline-GitLab/master/pipelines/pre-registry-scanning/files/index.html
	wget https://raw.githubusercontent.com/OzNetNerd/Deep-Security-Smart-Check-Pipeline-GitLab/master/pipelines/pre-registry-scanning/files/.gitlab-ci.yml
	wget https://raw.githubusercontent.com/OzNetNerd/Deep-Security-Smart-Check-Pipeline-GitLab/master/pipelines/pre-registry-scanning/files/docker-compose.yml
	git add .
	git commit -m 'Initial commit'
	git push
	```

5. Browse to the CI/CD pipeline and watch it perform its tasks.

6. When the pipeline finishes running, the image is ready for end-user consumption. Spin it up with the following command:

	```
	docker login <blessed_registry_hostname>:5000 -u <blessed_registry_username> -p <blessed_registry_password>
	docker pull <blessed_registry_hostname>:5000/safe-website
	BLESSED_REGISTRY_HOSTNAME=<blessed_registry_hostname> docker-compose up -d
	```

7. Browse to `http://<gitlab_hostname>:8080` to view the website.

8. Uncomment the `banner.jpg` line from the Dockerfile.
 
9. Commit and push the change.
	```
	git commit -am 'Added banner'
	git push
	```

10. Once the job fails, browse to the Smart Check UI and click "Scans" to examine the problem(s) Smart Check found.

	**Note:**  Because the change failed the pipeline, the modified `Dockerfile` will not be published. Therefore your image will continue to be safe. 