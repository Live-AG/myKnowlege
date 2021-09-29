

### Install GitLab

### Install GitLab Runner

1.	Add the official GitLab repository:
	
		curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
		
2. Install the latest version of GitLab Runner

		sudo apt install gitlab-runner

3. To install a specific version of GitLab Runner (if needed)

		apt-cache madison gitlab-runner 
		sudo apt install gitlab-runner=10.0.0
		
4. Register runner

		sudo gitlab-runner register
		
> Obtain a token:
>     -   For a [shared runner](https://docs.gitlab.com/ee/ci/runners/#shared-runners), have an administrator go to the GitLab Admin Area and click **Overview > Runners**
>     -   For a [group runner](https://docs.gitlab.com/ee/ci/runners/README.html#group-runners), go to **Settings > CI/CD** and expand the **Runners** section
>     -   For a [project-specific runner](https://docs.gitlab.com/ee/ci/runners/README.html#specific-runners), go to **Settings > CI/CD** and expand the **Runners** section

Note:
> Enter a description for the runner: `<empty>`
> Enter tags for the runner (comma-separated): `<empty>`