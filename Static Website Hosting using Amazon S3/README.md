# Hosting a Static Website using Amazon S3

Advantage of S3 is there are no Windows or Linux servers to manage, and no need to provision machines, install operating systems, or fine-tune web server configurations. There's also no need to manage storage infrastructure (such as SAN, NAS) because Amazon S3 provides practically limitless cloud-based storage. Fewer moving parts means fewer troubleshooting headaches.

## Steps to host a website:

1. [Create an S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html) by logging into the AWS Console (make sure the bucket name is unique globally).
2. [Upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/upload-objects.html) the index document and error document (optional) into the S3 bucket and make sure you have given read access to each uploaded file to Everyone in Access Control List (ACL) as they need to be accessed through internet.
3. Select the bucket -> Properties -> Static Website Hosting -> Edit -> Enable - static website hosting.
4. Provide the index file and error page file details under the required fields in the "host a static website" section.
5. Select hosting type as Host a static website and enter the index document and error document details which are uploaded into S3 bucket already under the respective fields. 
6. Select the Bucket -> Properties -> Static Website Hosting -> copy the Bucket website endpoint URL.

Now, users will be able to access the website using the website endpoint URL.

## Additional Information:

- For small, non-public websites, the Amazon S3 website endpoint is probably adequate. You can also use internal DNS to point to this endpoint.
- For a public-facing website, we recommend using a custom domain name instead of the provided Amazon S3 website endpoint. This way, users can see user-friendly URLs in their browsers.
- If you plan to use a custom domain name, your bucket name must match the domain name. For custom root domains (such as example.com), only Amazon Route 53 can configure a DNS record to point to the Amazon S3 hosted website.
```
Feel free to reach out if you need any further assistance or have additional requests! 😊
