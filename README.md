# AWS lightSail

## Description

#### The task is to launch the catalogUdacity project to the internet using Amazon Web Service with lightsail. A linux instance will be created and configured using the steps listed below.

## Logging into AWS and using Lightsail
1. #### Log into your Amazon account.
2. #### Create an instance
	A Lightsail instance is a Linux server running on a virtual machine inside an Amazon datacenter.
3. #### Choose an instance image: Ubuntu
	Lightsail supports a lot of different instance types. An instance image is a particular software setup, including an operating system and optionally built-in applications.
	For this project, you'll want a plain Ubuntu Linux image. There are two settings to make here. First, choose "OS Only" (rather than "Apps + OS"). Second, choose Ubuntu as the operating system.
4. #### Choose your instance plan.
	The instance plan controls how powerful of a server you get. It also controls how much money they want to charge you. For this project, the lowest tier of instance is just fine. And as long as you complete the project within a month and shut your instance down, the price will be zero.
5. #### Give your instance a hostname
	Every instance needs a unique hostname. You can use any name you like, as long as it doesn't have spaces or unusual characters in it. Your instance's name will be visible to you and to the project reviewer.
6. #### Wait for it to start up.
	It may take a few minutes for your instance to start up.
7. #### It's running
	Once your instance has started up, you can log into it with SSH from your browser.

	The public IP address of the instance is displayed along with its name. For intstance 54.84.49.254.

	Note: When you set up OAuth for your application, you will need a DNS name that refers to your instance's IP address. You can use the xip.io service to get one; this is a public service offered for free by Basecamp. For instance, the DNS name 54.84.49.254.xip.io refers to the server above.
	
	Explore the other tabs of this user interface to find the Lightsail firewall and other settings. You'll need to configure the Lightsail firewall as one step of the project.

	When you SSH in, you'll be logged as the ubuntu user. When you want to execute commands as root, you'll need to use the sudo command to do it.

## Configuring the AWS lightsail

1. ##### Updating And Managing software

```
sudo apt-get update
```

```
sudo apt-get upgrade
```

```
sudo apt-get autremove
```

2. ##### Configuring Uncomplicated Firewall (UFW)

```
sudo status
```

## Deny all incoming threw firewall
```
sudo ufw default deny incoming
```

## Allow all outgoing threw firewall
```
sudo ufw default allow outgoing
```

## Allow ssh
```
sudo ufw allow ssh
```

## Allow ssh on port 2200 with tcp
```
sudo ufw allow 2200/tcp
```

## Allow for basic http server
```
sudo ufw allow www
```

## Allow NTP
```
sudo ufw allow ntp
```

## Allow ntp on port 123
```
sudo ufw allow 123/udp
```

## Enable Firewall
```
sudo ufw enable
```

## See the status of the firewall
```
sudo ufw status
```

## Secure your server
##### Disable root and create user that has ability to use sudo. Eliminates a attack vector threw remote login. Some services already create an user with these capabilities. If so you can skip this step.

1. #### Create user Grader for review sudo permission.
	#### Create new user grader
	```
	sudo adduser grader
	```
	#### Pass: grader
2. #### Changing to grader terminal.
	```
	sudo su grader
	```
	```
	cd
	mkdir .ssh
	chmod 700 .ssh
	```
	```
	cd
	touch .ssh/authorized_keys
	chmod 600 .ssh/authorized_keys
	```
3. #### Switching back to Ubuntu terminal(can close and reopen window terminal(since grader hasnt the apropriate permissions yet))
	```
	sudo cat /etc/sudoers
	```
	```
	sudo touch /etc/sudoers.d/grader
	```
	```
	sudo nano /etc/sudoers.d/grader
	```
	WRITE:
	grader ALL=(ALL) NOPASSWD:ALL
	COMMAND:
	ctrl^x
	Y
	ENTER
4. #### Creating key pair

	#### If you switch back to grader terminal it will have pseudo access!
	As ubuntu user:
	```
	ssh-keygen
	```
	Location for file should be:
	/home/ubuntu/.ssh/PLACE_HOLDER
	Pass:catalog

	```
	cd
	sudo cat .ssh/PLACE_HOLDER.pub
	```
	As grader user:
	```
	cd 
	cd .ssh
	sudo nano authorized_keys
	```
	#### COPY the ssh-rsa key to authorized keys on ONE LINE.
	#### Follow the instructions to help copy and paste between terminals.
	##### To paste text into the browser-based SSH client

	1. Highlight text in your local desktop, then press Ctrl+C or Cmd+C to copy it to your local clipboard.

	2. In the bottom right corner of the browser-based SSH client, choose the clipboard icon. The browser-based SSH client clipboard text box appears.

	3. Click into the text box, then press Ctrl+V or Cmd+V to paste the contents from your local clipboard into the browser-based SSH client clipboard.

	4. Right-click any area on the SSH terminal screen to paste the text from the browser-based SSH client clipboard to the terminal screen.

	##### To copy text from the browser-based SSH client

	1. Highlight text on the terminal screen.

	2. In the bottom right corner of the browser-based SSH client, choose the clipboard icon. The browser-based SSH client clipboard text box appears.

	3. Highlight the text that you want to copy, then press Ctrl+C or Cmd+C to copy the text to your local clipboard. You can now paste the copied text anywhere in your local desktop.
	#### If reference needed see first link in WebBio


5. #### Disable base logins
6. #### Restart ssh services.
```
sudo service ssh restart
```

## Contribuition

##### To contribuite to this project the person must follow the guidelines mentioned in **Usage** and send a written email to the moderator of the source code hugo.smits1995@gmail.com 

## Licensing info

##### The following project uses the [MIT license](https://opensource.org/licenses/MIT)

## WebBio

- ##### Full Stack Web Developer Nanodegree Udacity Lesson 5 From Linux Server Configuration -  "Get Started on LightSail"
- ##### https://lightsail.aws.amazon.com/ls/docs/en/articles/lightsail-how-to-connect-to-your-instance-virtual-private-server
- ##### https://aws.amazon.com/pt/premiumsupport/knowledge-center/new-user-accounts-linux-instance/
- ##### https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html

