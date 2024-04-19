# Creating a Webserver Using AWS EC2

You can create a webserver by installing web packages on a Linux EC2 instance. Follow these steps:

1. **Log in to AWS Console:**
   - Enter your credentials and log in to the AWS console home.
   - Navigate to the EC2 Dashboard.

2. **Create an EC2 Instance:**
   - Create an EC2 machine with the desired hardware specifications and choose an Amazon Machine Image (AMI) (e.g., Ubuntu, Amazon Linux, etc.).

3. **Installation of web packages:**
   - There are two ways to turn a Linux machine into a webserver:
     1. **Manual Installation:**
        - Log in to the Linux machine.
        - Update the package lists on your instance:
          - For Amazon Linux: `sudo yum update`
          - For Ubuntu: `sudo apt update`
        - Install the necessary software (e.g., Apache, Nginx, or any other web server):
          - For Apache on Amazon Linux: `sudo yum install httpd`
          - For Nginx on Ubuntu: `sudo apt install nginx`
        - Start the service and enable it to start on boot:
          - For Apache: `sudo systemctl start httpd` and `sudo systemctl enable httpd`
          - For Nginx: `sudo systemctl start nginx` and `sudo systemctl enable nginx`
        - Create or copy your website files (HTML, CSS, JavaScript, etc.) to the appropriate directory:
          - Apache: `/var/www/html/`
          - Nginx: `/var/www/`

4. **Using Bootstrap Scripts:**
   - Alternatively, you can provide bootstrap scripts under USER DATA at the time of creating the EC2 instance.
   - After creating the security group, navigate to advanced settings and add the following bootstrap script in USER DATA:

```bash
#!/bin/sh
sudo yum update -y
sudo yum install httpd -y
sudo service httpd start
chkconfig httpd on
cd /var/www/html
cat >> index.html << EOF
<html><body> This is a sample for a webserver on EC2 </body></html>
EOF
