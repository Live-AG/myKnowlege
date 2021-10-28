# Ansible

Установка на Ubuntu 16.04: 

	sudo apt-add-repository ppa:ansible/ansible 
	sudo apt-get update 
	sudo apt-get install ansible 

Установка на CentOS 7: 

	sudo yum install epel-release 
	sudo yum install ansible


source:
https://linuxhandbook.com/ci-with-gitlab-jenkins-and-sonarqube/

Tokens:
	- SonarQube (PipelineProject): c95e48c4b5594913762176f980b0f67d432c6474
	- GitLab (PipelineProject): yxsgVouDmjxxPRPsbD2k
	- Sonar Scaner token: bfe69028218abcf201fffe8566dca2b7 
URL: http://192.168.1.214:8080/project/First%20Pipeline


### Step 1. Configurations at Sonarqube

Install [[SonarQube Linux setup|SonarQube]]

We require **server authentication token** from SonarQube, that we later pass to Jenkins. This token gives access to Jenkins, to push Jenkins builds at SonarQube for code anaylsis.

-   Go to **My Account** > **Security**
-   At **Tokens** block, enter any text to generate a token.
-   Keep the copy of the token

Now, we will create a Project where all the code analysis reports are published.

-   Go to **Administration** > **Projects** > **Management**
-   Click on **Create Project**
-   Create the project with your Project_name and Project_key. Copy the project name and key. We will pass this credentials at Jenkins configuration later on.

### Step 2. Configuration at GitLab

Install [[GitLab setup|GitLab]]

We also need GitLab user’s Access Tokens that we later pass at Jenkins. This is used to authenticate GitLab user’s repository url, from where Jenkins pull the codes from.

-   Go to **User Settings** form Settings menu.
-   Go to **Access Tokens**
-   Create a personal access token by adding any unique name(**Name**) and token expiry date(**Expires at**). Also set the **Scopes** to api- Full access.

Network setting:
I looked all over the Admin page for something about **allow fetch local resources**. I found **Admin -> Settings -> Network -> Outbound Requests -> Allow requests to the local network from hooks and services**

### Step 3. Configuration at Jenkins
Install [[Jenkins setup|Jenkins]]

We need to configure GitLab and SonarQube at Jenkins web panel. For this, we need to install some necessary plugins.

-   Login to Jenkins
-   Go to **Manage Jenkins** > **Manage Plugins**
-   At **Available** tab, search for GitLab and SonarQube and install the following plugins:
-   GitLab Hook Plugin
-   GitLab Plugin
-   Git
-   SonarQube Scanner for Jenkins

We require **SonarQube Scanner** to be installed at “Jenkins server” which actually starts code analysis and publish the reports to project at SonarQube.

To install SonarQube Scanner, you can use the following commands:

```
$ wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-3.3.0.1492-linux.zip
$ unzip sonar-scanner-cli-3.3.0.1492-linux.zip 
$ cd sonar-scanner-3.3.0.1492-linux $ pwd 
```

Copy the location. We will need to add this location (as SonarQube Scanner Installation Home Folder) at Jenkins Configuration.

We will also configure sonar-scanner properties file at add SonarQube server:

```
$ cd conf 
$ vi sonar-scanner.properties
```

Uncomment “sonar.host.url” and add your SonarQube server URL

[![](https://linuxhandbook.com/content/images/2020/06/sonar-scanner-conf.png)](https://linuxhandbook.com/content/images/2020/06/sonar-scanner-conf.png)

[![](https://i1.wp.com/linuxhandbook.com/wp-content/uploads/sonar-scanner-conf-3.png?ssl=1)](https://i1.wp.com/linuxhandbook.com/wp-content/uploads/sonar-scanner-conf-3.png?ssl=1)

Configure the SonarQube server

Now we will configure GitLab and SonarQube at Jenkins.

-   Go to **Manage Jenkins** > **Configure System**
-   At **SonarQube servers** tab, enter your SonarQube server URL and the **Server authentication token** generated at SonarQube before.

Preview of adding SonarQube at Jenkins:

[![Adding SonarQube to Jenkins for CI setup](https://linuxhandbook.com/content/images/2020/06/jenkins-sonar.png)](https://linuxhandbook.com/content/images/2020/06/jenkins-sonar.png)

Adding SonarQube to Jenkins

Now, go to GitLab tab and add your GitLab Server URL at **GitLab host URL**

At **Credentials**, we need GitLab API token for accessing GitLab. Click on **Add** and select **Jenkins:** **Jenkins Credentials Provider**

[![](https://linuxhandbook.com/content/images/2020/06/jenkins-gitlab_LI.jpg)](https://linuxhandbook.com/content/images/2020/06/jenkins-gitlab_LI.jpg)

At **Kind**, select **GitLab API Token** from drop down list. Enter your API Token generated at GitLab before. Add the Token with a unique ID.

[![](https://linuxhandbook.com/content/images/2020/06/jenkins-gitlab-api.png)](https://linuxhandbook.com/content/images/2020/06/jenkins-gitlab-api.png)

Also, Ensure that you have correct Jenkins Location at **Jenkins Location** tab.

[![](https://linuxhandbook.com/content/images/2020/06/jenkins-jenkins.png)](https://linuxhandbook.com/content/images/2020/06/jenkins-jenkins.png)

After added GitLab and SonarQube succesfully, we also need to add SonarQube Scanner configurations.

-   Go to **Manage Jenkins** > **Global Tool Configuration**
-   At **SonarQube Scanner** tab, click on **SonarQube Scanner Installations**
-   Untick **Install automatically**, and add your SonarQube Installation Home Folder.

[![SonarQube scanner setup in Jenkins](https://linuxhandbook.com/content/images/2020/06/jenkins-sonarscanner.png)](https://linuxhandbook.com/content/images/2020/06/jenkins-sonarscanner.png)

My SonarQube scanner home folder was /opt/sonar-scanner