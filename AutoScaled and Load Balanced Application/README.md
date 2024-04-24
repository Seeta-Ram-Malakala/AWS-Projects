# Creating an Auto Scalable and Load Balanced Application

## Introduction

You can set up a load-balanced application by registering your Auto Scaling group with an Elastic Load Balancing load balancer. To distribute incoming traffic across your healthy Amazon EC2 instances, use Elastic Load Balancing in conjunction with Amazon EC2 Auto Scaling. This improves your application's availability and scalability. Elastic Load Balancing can be enabled across several Availability Zones to improve your applications' fault tolerance.

## Steps to Create a Scalable and Load Balanced Application

### Step 1: Set up a Launch Template or Launch Configuration

 - You can create a launch template or launch configuration based on your requirements.

 - Launch template or launch configuration contains the information regarding the AMI, Volumes, Security Group, Instance type, Key Pairs etc.

 - For example, you can use the following code to create a web server using bash code under USER DATA in advanced settings while creating Launch Configuration or Launch Template:

```bash
#!/bin/sh

sudo yum update -y
sudo yum install httpd -y
sudo service httpd start
chkconfig httpd on
cd /var/www/html
cat >> index.html << EOF
<html><body> this is ec2 instance </body></html>
EOF
```
 - You can also select an existing launch template available in the EC2 dashboard.

### Step 2: Create an Autoscaling Group

- On the Choose launch template or configuration page, enter a name for your Auto Scaling group.
- [Launch template only] For Launch template, choose whether the Auto Scaling group uses the default, the latest, or a specific version of the launch template when scaling out.
- Choose Next.
- The Choose instance launch options page appears, allowing you to choose the VPC network settings you want the Auto Scaling group to use and giving you options for launching On-Demand and Spot Instances (if you chose a launch template).
- In the Network section, for VPC, choose the VPC that you used for your load balancer. If you chose the default VPC, it is automatically configured to provide internet connectivity to your instances. This VPC includes a public subnet in each Availability Zone in the Region.
- For Availability Zones and subnets, choose one or more subnets from each Availability Zone that you want to include, based on which Availability Zones the load balancer is in. For more information, see Considerations when choosing VPC subnets.
- [Launch template only] In the Instance type requirements section, use the default setting to simplify this step. (Do not override the launch template.) you will launch only On-Demand Instances using the instance type specified in your launch template.
- Choose Next to go to the Configure advanced options page.
- To attach the group to an existing load balancer, in the Load balancing section, choose Attach to an existing load balancer. You can choose Choose from your load balancer target groups or Choose from Classic Load Balancers. You can then choose the name of a target group for the Application Load Balancer or Network Load Balancer you created or choose the name of a Classic Load Balancer.
- (Optional) To use Elastic Load Balancing health checks, for Health checks, choose ELB under Health check type.
- When you have finished configuring the Auto Scaling group, choose Skip to review.
- On the Review page, review the details of your Auto Scaling group. You can choose Edit to make changes. When you are finished, choose Create Auto Scaling group.

### Step 3: Verify that Your Load Balancer is Attached

- From the Auto Scaling groups page of the Amazon EC2 console, select the check box next to your Auto Scaling group.
- On the Details tab, Load balancing shows any attached load balancer target groups or Classic Load Balancers.
- On the Activity tab, in Activity history, you can verify that your instances launched successfully. The Status column shows whether your Auto Scaling group has successfully launched instances. If your instances fail to launch, you can find troubleshooting ideas for common instance launch issues in Troubleshoot Amazon EC2 Auto Scaling.
- On the Instance management tab, under Instances, you can verify that your instances are ready to receive traffic. Initially, your instances are in the Pending state. After an instance is ready to receive traffic, its state is Inservice. The health status column shows the result of the Amazon EC2 Auto Scaling health checks on your instances. Although an instance may be marked as healthy, the load balancer will only send traffic to instances that pass the load balancer health checks.
- Verify that your instances are registered with the load balancer. Open the Target groups page of the Amazon EC2 console. Select your target group, and then choose the Targets tab. If the state of your instances is initial, it's probably because they are still in the process of being registered, or they are still undergoing health checks. When the state of your instances is healthy, they are ready for use.

### Step 4: Few more configurations

- Amazon EC2 Auto Scaling determines whether an instance is healthy based on the status of the health checks that your Auto Scaling group uses. If you enable load balancer health checks and an instance fails the health checks, your Auto Scaling group considers the instance unhealthy and replaces it. For more information, see Health checks.
- You can expand your application to an additional Availability Zone in the same Region to increase fault tolerance if there is a service disruption. For more information, see Add Availability Zones.
- You can configure your Auto Scaling group to use a target tracking scaling policy. This automatically increases or decreases the number of instances as the demand on your instances changes. This allows the group to handle changes in the amount of traffic that your application receives. For more information, see Target tracking scaling policies.

### Step 5: Connecting to the Application

- You can connect to the application using the load balancer DNS through the internet.
