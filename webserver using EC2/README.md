# Creating a Webserver Using AWS EC2

You can create a webserver by installing web packages on a Linux EC2 instance. Follow these steps:

1. **Log in to AWS Console:**
   - Enter your credentials and log in to the [AWS console home](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3FhashArgs%3D%2523%26isauthcode%3Dtrue%26state%3DhashArgsFromTB_eu-north-1_9edbdae3fc9563a1&client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fcanvas&forceMobileApp=0&code_challenge=Gj8I0x32h8kxuebTM8sKjgWNP9UlXqI2FgNrk9NnReA&code_challenge_method=SHA-256).
   - Navigate to the EC2 Dashboard.

2. **Create an EC2 Instance:**
   - Create an [EC2 machine](https://docs.aws.amazon.com/efs/latest/ug/gs-step-one-create-ec2-resources.html) with the desired hardware specifications and choose an Amazon Machine Image (AMI) (e.g., Ubuntu, Amazon Linux, etc.).

3. **Installation of web packages:**
   - There are two ways to turn a Linux machine into a webserver:
     1. **Manual Installation:**
        - [Connect to Linux machine](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-to-linux-instance.html).
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
     2. **Using Bootstrap Scripts:**
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
