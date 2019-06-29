# Trend Micro Smart Check CI/CD pipelines with GitLab
## Prerequisites

1. Set up your [Smart Check environment](https://github.com/OzNetNerd/Smart-Check-Demo)

2. Set up your [Gitlab environment](https://github.com/OzNetNerd/Packer-Gitlab)

	Note: Configure your GitLab instance's security group to allow:
		
	* Any traffic from your IP
	* Inbound traffic from any address on port TCP 5000 

## Instructions

Navigate to the [pipelines](https://github.com/OzNetNerd/Smart-Check-Pipeline-GitLab/tree/master/pipelines/) directory and follow the instructions for each of the labs you'd like to run.

**Note:** Unless otherwise instructed, all of the commands in the pipeline examples should be executed on the GitLab instance.

### Find & Replace

All pipeline examples require user-specific information, such as your Docker registry address. These will be called out at the start of each example so that you can find & replace them all before running through the instructions.

Doing so will allow you to flow through the instructions with ease.