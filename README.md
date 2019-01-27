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



## Contribuition

##### To contribuite to this project the person must follow the guidelines mentioned in **Usage** and send a written email to the moderator of the source code hugo.smits1995@gmail.com 

## Licensing info

##### The following project uses the [MIT license](https://opensource.org/licenses/MIT)

## WebBio

- ##### Full Stack Web Developer Nanodegree Udacity Lesson 5 From Linux Server Configuration -  "Get Started on LightSail"

