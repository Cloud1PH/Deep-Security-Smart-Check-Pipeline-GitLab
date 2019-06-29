# Description

Use Smart Check to scan images which have already been deployed into production. While this example does not make use of a CI/CD pipeline, it replicates a real-world use case.

When used it in a real-world environment, it is highly recommended that issues identified by Smart Check be remedied without delay.

# Instructions
## Find & Replace

* **<registry_hostname>** - Docker registry hostname 
* **<registry_username>** - Docker registry username (default: testuser)
* **<registry_password>** - Docker registry password (default: testpassword) 

## Upload image to registry

1. Download an image:

	```
	docker pull vulnerables/web-dvwa
	```

2. Tag it with the address of your Docker registry:

	```
	docker tag vulnerables/web-dvwa <registry_hostname>:5000/web-dvwa
	```

3. Log into your registry:

	```
	docker login -u <registry_username> -p <registry_password> <registry_hostname>:5000
	```

4. Push it to your registry:

	```
	docker push <registry_hostname>:5000/web-dvwa
	```

## Scan registry

Now that the image has been uploaded to your registry, it's time to have Smart Check scan it.

1. Log into the Smart Check UI.

2. Create a new registry with the following details:

	* **Name:** Reg
	* **Description:** Private registry
	* **Registry Type:** Generic
		* **Registry Host:** <registry_hostname>:5000
		* Tick the **"Skip registry certificate validation (insecure)"** box
	* **Registry User:** <registry_username>
	* **Registry Password:** <registry_password>