# Dual Registry Pipeline (Pre-Registry Scanning)
## Description

This example replicates a typical DevOps pipeline. Upon modification, Docker images are sent to a "dirty" registry which is **only available to developers and Smart Check**.  Smart Check will examine the image's integrity and if an issue is found, it will fail the pipeline. 

If no issues are found, the pipeline progresses and the image is placed into a "blessed" registry. This registry is available to users for consumption.

This two staged approach ensures that only known safe images are available for consumption.        

# Instructions
## Find & Replace

* **<gitlab_hostname>** - Gitlab hostname  
* **<dirty_registry_hostname>** - Dirty Docker registry hostname 
* **<dirty_registry_username>** - Dirty Docker registry username (default: testuser)
* **<dirty_registry_password>** - Dirty Docker registry password (default: testpassword)
* **<blessed_registry_hostname>** - Blessed Docker registry hostname 
* **<blessed_registry_username>** - Blessed Docker registry username (default: testuser)
* **<blessed_registry_password>** - Blessed Docker registry password (default: testpassword)
* **<dssc_hostname>** - Smart Check hostname
* **<dssc_username>** - Smart Check username
* **<dssc_password>** - Smart Check password

## Set up your project

1. Start an additional registry:
	```
	make REGISTRY_NAME=dirty-registry REGISTRY_PORT=5001 start-registry
	```

2. Log into the GitLab UI and create a project:
	* **Name:** Dual Registry Pipeline
	* **Visibility Level:** Private

	If you're asked to add an SSH, click **"Don't show again"** 
 
3. 	Navigate to "Settings -> CI/CD". Make the following changes:
	1. Turn "Auto DevOps" off
	2. Configure the following variables:
		* **BLESSED_REGISTRY_HOSTNAME:** <blessed_registry_hostname>
		* **BLESSED_REGISTRY_USERNAME:** <blessed_registry_username>
		* **BLESSED_REGISTRY_PASSWORD:** <blessed_registry_password>
		* **DIRTY_REGISTRY_HOSTNAME:** <dirty_registry_hostname>
		* **DIRTY_REGISTRY_USERNAME:** <dirty_registry_username>
		* **DIRTY_REGISTRY_PASSWORD:** <dirty_registry_password>		
		* **DSSC_HOSTNAME:** <dssc_hostname>
		* **DSSC_USERNAME:** <dssc_username>
		* **DSSC_PASSWORD:** <dssc_password>
		
		Click "Save variables"

4. Set up git and clone the project:

	```
	git config --global user.name "Administrator"
	git config --global user.email "admin@example.com"
	git clone http://<gitlab_hostname>/root/dual-registry-pipeline.git
	```
	
	When prompted, enter your GitLab username (`root`) and password.
		
5. Navigate to the new directory and pull down the files required for this example:
	
	```
	cd dual-registry-pipeline
	wget https://raw.githubusercontent.com/OzNetNerd/Deep-Security-Smart-Check-Pipeline-GitLab/master/pipelines/dual-registry-pipeline/files/Dockerfile
	wget https://raw.githubusercontent.com/OzNetNerd/Deep-Security-Smart-Check-Pipeline-GitLab/master/pipelines/dual-registry-pipeline/files/index.html
	wget https://raw.githubusercontent.com/OzNetNerd/Deep-Security-Smart-Check-Pipeline-GitLab/master/pipelines/dual-registry-pipeline/files/.gitlab-ci.yml
	wget https://raw.githubusercontent.com/OzNetNerd/Deep-Security-Smart-Check-Pipeline-GitLab/master/pipelines/dual-registry-pipeline/files/docker-compose.yml
	git add .
	git commit -m 'Initial commit'
	git push
	```

6. Browse to the CI/CD pipeline and watch it perform its tasks.

7. When the pipeline finishes running, the image is ready for end-user consumption. Spin it up with the following command:

	```
	docker login <blessed_registry_hostname>:5000 -u <blessed_registry_username> -p <blessed_registry_password>
	docker pull <blessed_registry_hostname>:5000/safe-website
	BLESSED_REGISTRY_HOSTNAME=<blessed_registry_hostname> docker-compose up -d
	```

8. Browse to `http://<gitlab_hostname>:8080` to view the website.

9. Uncomment the `banner.jpg` line from the Dockerfile.
 
10. Commit and push the change.
	```
	git commit -am 'Added banner'
	git push
	```

11. Once the job fails, browse to the Smart Check UI and click "Scans" to examine the problem(s) Smart Check found.

	**Note:**  Because the change failed the pipeline, the modified `Dockerfile` will not be published. Therefore your image will continue to be safe. 