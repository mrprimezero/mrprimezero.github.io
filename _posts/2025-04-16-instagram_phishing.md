---
title: Instagram Phishing 101: Understanding the Mechanics (and Defenses)
date:2025-04-16 16:47:00 +0530
categories: [Social-Engineering, Phishing]
tags: [apache, web-server, phishing, social-engineering, mysql]
---

# The phishing Threat
Phishing is the most used tactic in a social-engineering attacks, one of the main reasons being `effectiveness`. No matter how advanced or how far is cyber security is gone, The most vulnerable system in this world will always be `Humans`. The main focus of a phishing attack is to create a sense of fear and urgency, which is why it is the most effective hacking method.

## Social Media as a Target: Why Instagram?
Social media accounts contain the most valuable thing a hacker desires `Data`, Social accounts contain so much secrecy and privacy which a hacker could then use to blackmail a `victim` into demanding a ransom. And the victim would also pay the ransom due to their `fear`. In this post I will be demonstrating how to obtain Instagram credentials of a victim(with their consent), I have chosen Instagram due to it being the most used social platform among Gen-Z. 

## The Goal of this Post
This post is published strictly for educational purposes only. I do not condone any Malicious or Illegal activity. The main focus is to teach how easily a phishing attack could occur and how we could possibly defend against it.

# How Instagram Phishing works
Phishing is not something that is overly complicated it mostly plays with a persons emotions. So in order to achieve the outcome we desire we must make things look as authentuc as possible. In this demonstration we will be using a phishing page created by yours truly PrimeZero(ME).

## Instagram Phishing Repository
I have created an Instagram which is identical to the original with some modifications to have an edge on our attempt at `Phishing`. Let's clone the GitHub repository.

```console
git clone https://github.com/mrprimezero/Instagram-Phishing-Page
```

This repository contains a webpage and an e-mail template which we can send out to the target.

# Setting up the Phishing Page
For our web server to work we will be using `apache`, `ngrok` and `mysql`. I recommend using Kali-Linux since we have everything we need there and also cause it's cooler. So lets set them up now.

## MySQL
We are going to set up MySQL and this will store our captured `username` and `password` in a database called capturelogin.

Before we begin let's check the status of MySQL in our system
```console
sudo systemctl status mysql
```
This will let us know if MySQL services are active. If the status shows as disbaled, start services by issuing this command:

```console
sudo systemctl start mysql
```
And now let's login to our MySQL service.

```console
sudo mysql -u root -p
```
Enter in your root password here.

### Creating the Database
Now let's create a Database name `capturelogin`. Note: You may name it anything as you desire.

```console
CREATE DATABASE capturelogin;
```
After creating the database let's create a user and a password and grant that user privilges to the database,
```console
CREATE USER 'logger'@'localhost' IDENTIFIED BY 'logmaster';
```
```console
GRANT ALL PRIVILEGES ON capturelogin.* TO 'logger'@'localhost';
```
and now let's flush privileges to keep the privilege information up-to-date,
```console
FLUSH PRIVILEGES;
```
Now we have to use that database we created to create a table to add our victim details.

```console
USE capturelogin;
```
Now let's create the table to record our login details.
```console
CREATE TABLE logins (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
And that's all we have to do with MySQL, But don't close the MySQL shell yet. 

## Apache Server
Launch a new terminal instance and check whether Apache services are active,
```console
sudo systemctl status apache2
```
if the services are active stop the services for now as you might run into problems,
```console
sudo systemctl stop apache2
```

now you are gonna have to place the `index.html`, `style.css` and the `info.php` files in `/var/www/html`

### Configuring PHP File
And now for the most important part, we will be configuring the `info.php` so it can speak with our database.
```console
// Database credentials
$servername = "localhost";
$username = "logger";
$password = "logmaster";
$dbname = "capturelogin";
```
The only thing that needs to be done is change the database credentials and we are set.

now let's start the apache server,
```console
sudo systemctl start apache2
```
### Ngrok
