# How to Install GitLab on an Ubuntu EC2 Instance
## Introduction
GitLab is a powerful web-based platform for version control, continuous integration, and collaboration on software projects. In this guide, we'll walk you through the process of setting up GitLab on an Ubuntu EC2 instance, making it easier than ever to manage your code and streamline your development workflow.

## Prerequisites
Before we dive into the installation, let's make sure you have everything in place:

1- **AWS EC2 Instance:** Ensure you've already launched an AWS EC2 instance running Ubuntu, and you have SSH access to it.

2- **Permissions:** Make sure you have the necessary permissions to perform administrative tasks on your EC2 instance.

## Step 1: Update System Packages

The first step is to connect to your EC2 instance via SSH and update your system packages. This ensures that you have the latest package information and updates.
```console
sudo apt update
sudo apt upgrade -y
```
## Step 2: Install Dependencies

GitLab relies on several dependencies, and we need to install them. This includes packages like **curl**, **openssh-server**, **ca-certificates**, **tzdata**, and **perl**.
```console
sudo apt install -y curl openssh-server ca-certificates tzdata perl
```


## Step 3:  Optional - Install and Configure Postfix

If you want to enable email notifications from GitLab for account recovery and notifications, you can install and configure Postfix. It's a recommended step for better communication.
```console
sudo apt install -y postfix
```
During the Postfix configuration, select "Internet Site" as the mail server configuration type, and provide your system's fully qualified domain name (FQDN).

## Step 4: Install GitLab Package

GitLab offers an official package repository that we'll add to our system. This makes it easy to install and manage GitLab.

```console
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
```

## Step 5: Install GitLab CE

Now, let's install GitLab Community Edition (CE):

```console
sudo apt install -y gitlab-ce
```

## Step 6: Configure GitLab

With GitLab installed, it's time to configure it to your liking. Open the GitLab configuration file:
```console
sudo nano /etc/gitlab/gitlab.rb
```
Inside this file, you can customize settings such as the external URL. Save the file and exit the text editor.

## Step 7: Reconfigure GitLab

After making changes to the configuration file, reconfigure GitLab to apply the settings:
```console
sudo gitlab-ctl reconfigure
```
## Step 8: Access GitLab

Now that GitLab is set up, you can access it via a web browser by entering the URL or IP address you specified in the external_url setting. Use the following login credentials:


**-** **Username:** root  
**-** **Password:** The initial root password can be found in:
```console
sudo cat /etc/gitlab/initial_root_password
```

## Step 9: Change the Root Password

For security reasons, change the root password once you're logged in:

1- Go to "User Settings" > "Change password" and follow the prompts to set a new password.


## Step 10: Optional - Set Up SSL/TLS (HTTPS)

For enhanced security, consider configuring SSL/TLS (HTTPS) for your GitLab instance using a valid SSL certificate. You can use Let's Encrypt or a commercial certificate for this purpose.

## Step 11: Login into the EC2 Instance 

## Step 12: Simply download one of the binaries for your system: 

```console
sudo curl -L -o /usr/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"
```

## Step 13: Give it permission to execute:

```console
sudo chmod +x /usr/bin/gitlab-runner
```

## Step 14: Create a GitLab CI user:

```console
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```

## Step 15: Install and run as service:
```console
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```

## Step 16: Create runner in Project > Setting > CI/CD > Runner > Expand

## Step 17: Now You'll be seeing command, just run them on your EC2 Instance, and choose shell. You'll get the Congrats messege on your runner page, and runner get the green sign in front of it.

## Step 18: If you're getting error:
ERROR: Job failed: prepare environment: Process exited with status 1. 
Check https://docs.gitlab.com/runner/shells/index.html#shell-profile-loading 
for more information
```console
find / -name .bash_logout
sudo rm -r /home/gitlab-runner/.bash_logout
sudo rm -r /home/<username>/.bash_logout
```



## Conclusion:
Congratulations! You've successfully installed GitLab on your Ubuntu EC2 instance. With GitLab, you can now efficiently manage your repositories and collaborate with your team, streamlining your development processes.

Don't forget to regularly back up your GitLab instance to ensure the safety of your valuable data.
