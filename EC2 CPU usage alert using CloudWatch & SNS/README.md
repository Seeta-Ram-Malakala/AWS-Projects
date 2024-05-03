# EC2 CPU Usage Alerting using CloudWatch and SNS

### What is AWS CloudWatch:

CloudWatch is a service that helps in Monitoring, Alerting, Reporting, and Logging the AWS services. You can utilize it to understand more about the functionality, health, and performance of your AWS applications and resources. In addition to gathering and monitoring log files and analytics, CloudWatch also creates alerts to notify you under particular conditions.

### Advantages of AWS CloudWatch:

- **Comprehensive Monitoring:** allows you to monitor various AWS resources such as EC2 instances, RDS databases, Lambda functions, and more. You get a unified view of your entire AWS infrastructure.
- **Real-Time Metrics:** It provides real-time monitoring of metrics, allowing you to respond quickly to any issues or anomalies that might arise.
- **Automated Actions:** You can set up automated actions like triggering an Auto Scaling group to scale in or out based on certain conditions.
- **Log Insights:** Lets you analyze and search log data from various AWS services, making it easier to troubleshoot problems and identify trends.
- **Dashboards and Visualization:** Create custom dashboards to visualize your application and infrastructure metrics in one place, making it easier to understand the overall health of your system.

### Problem Solving with AWS CloudWatch:

**CloudWatch** helps address several critical challenges, including:

- **Resource Utilization:** Tracking resource utilization and performance metrics to optimize your AWS infrastructure efficiently.
- **Proactive Monitoring:** Identifying and resolving issues before they impact your applications or users.
- **Troubleshooting:** Analyzing logs and metrics to troubleshoot problems and reduce downtime.
- **Scalability:** Automatically scaling resources based on demand to ensure optimal performance and cost efficiency.

### Practical Use Cases of AWS CloudWatch:

- **Auto Scaling:** CloudWatch can trigger Auto Scaling actions based on defined thresholds. For example, you can automatically scale in or out based on CPU utilization or request counts.
- **Resource Monitoring:** Monitor EC2 instances, RDS databases, DynamoDB tables, and other AWS resources to gain insights into their performance and health.
- **Application Insights:** Track application-specific metrics to monitor the performance of your applications and identify potential bottlenecks.
- **Log Analysis:** Use CloudWatch Logs Insights to analyze log data, identify patterns, and troubleshoot issues in real-time.
- **Billing and Cost Monitoring:** CloudWatch can help you monitor your AWS billing and usage patterns, enabling you to optimize costs.

 ### What is SNS:
**Amazon Simple Notification Service (Amazon SNS)** is a service that provides message delivery from publishers to subscribers (also known as producers and consumers). Publishers communicate asynchronously with subscribers by sending messages to a topic, which is a logical access point and communication channel. Clients can subscribe to the SNS topic and receive published messages using a supported endpoint type, such as Amazon Data Firehose, Amazon SQS, AWS Lambda, HTTP, email, mobile push notifications, and mobile text messages (SMS).

## Steps to create EC2 CPU alerting:
1. Create an EC2 instance with the desired specifications from the EC2 dashboard after logging into your [AWS console](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3FhashArgs%3D%2523%26isauthcode%3Dtrue%26state%3DhashArgsFromTB_eu-north-1_1cf34c1de18c788f&client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fcanvas&forceMobileApp=0&code_challenge=WNQs19ziT-oE8HgHu7noGU8Xw4OfD4FEZjqlZxQLyMI&code_challenge_method=SHA-256).
2. [Create an SNS topic](https://docs.aws.amazon.com/sns/latest/dg/sns-create-topic.html) for the alarm to get notified when the CPU utilization exceeds the given threshold.
3. [Create an alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_AlarmAtThresholdEC2.html) in the CloudWatch dashboard with the metric as CPU utilization and mention the threshold as 80%. Add the SNS topic which created earlier.
4. Connect to the EC2 instance and create a Python file using VI editor/ VIM editor/ Nano editor with the following Python code to increase the CPU utilization forcefully.
```
import time
import math 
import sys 
import multiprocessing
 
def generate_cpu_load(interval=int(sys.argv[1]),utilization=int(sys.argv[2])):
    "Generate a utilization % for a duration of interval seconds"
    start_time = time.time()
    for i in range(0,int(interval)):
        print("About to do some arithmetic")
        while time.time()-start_time < utilization/100.0:
            a = math.sqrt(64*64*64*64*64)
        print(str(i) + ". About to sleep")
        time.sleep(1-utilization/100.0)
        start_time += 1
 
#----START OF SCRIPT
if __name__=='__main__':
    print("No of cpu:", multiprocessing.cpu_count())
    if len(sys.argv)>2:
        processes = []
        for _ in range (multiprocessing.cpu_count()):
            p = multiprocessing.Process(target =generate_cpu_load)
            p.start()
            processes.append(p)
        for process in processes:
            process.join()        
    else:
        print("Usage:\n python %s interval utilization"%__file__)
```
5. Run the Python script which was saved earlier.
6. Observe the CPU utilization graph increases gradually and exceeds the threshold, which is 80%.
7. You will receive an SNS notification saying that CPU utilization exceeded 80% to your email/mobile number based on the SNS topic you created.
 
