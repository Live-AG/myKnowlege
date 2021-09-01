## Installation from

https://about.gitlab.com/install/#ubuntu


### Install and configure the necessary dependencies

    sudo apt-get update
    sudo apt-get install -y curl openssh-server ca-certificates tzdata perl

### Add the GitLab package repository and install the package

Add the GitLab package repository.

    curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash

Next, install the GitLab package.

    sudo EXTERNAL_URL="https://gitlab.example.com" apt-get install gitlab-ee

3. Browse to the hostname and login

Unless you provided a custom password during installation, a password will be randomly generated and stored for 24 hours in /etc/gitlab/initial_root_password. Use this password with username root to login.

See our documentation for detailed instructions on installing and configuration.
4. Set up your communication preferences

Visit our email subscription preference center to let us know when to communicate with you. We have an explicit email opt-in policy so you have complete control over what and how often we send you emails.

Twice a month, we send out the GitLab news you need to know, including new features, integrations, docs, and behind the scenes stories from our dev teams. For critical security updates related to bugs and system performance, sign up for our dedicated security newsletter.

Important note: If you do not opt-in to the security newsletter, you will not receive security alerts.
5. Recommended next steps

After completing your installation, consider the recommended next steps, including authentication options and sign-up restrictions.