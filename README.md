# CloudResume-Project
Deploying my resume with AWS

I have been working to transition into a tech career for some time now and like most people, I spent tons of time, money, and effort stressing over certifications that I was led to believe would be the key to me landing a role in the dynamic tech industry. After having achieved the certifications that I thought would change everything, I started applying to jobs and began realizing that while these certs showed I was a quick-learner and were "proof" that I had the necessary knowledge to do the job, I still did not have much to offer in terms of anything that I had built. My cybersecurity and security engineer certifications mostly just allowed me to show screen shots of walk throughs that I had done during the course of my learning. So, I decided to create something to show to hiring managers. But what? I hate to admit that it took me sometime to determine what to create. In the end, it seemed fairly obvious.... A resume.

---
---

In this project, I am going to walk through the steps I took to create this resume and the things I learned along the way.

I will use a number of programming languages and AWS services in this project:
Programming = HTML, CSS, Javascript, Python,
AWS Services = S3, Route 53, CloudFront, Certificate Manager

First things first, you need a few things to get started:
- An AWS account with access to the console
- Basic understanding of code
- A resume that you can deploy (needs to be html with some css) so you have something nice to show off when hiring managers visit

If you do not have a resume ready to deploy, I suggest searching on youtube or medium for instruction/inspiration! It can be very basic at first and you can update it later.

Once you have your basic resume ready to deploy we are going to follow these steps:
1. Upload your resume to AWS S3 bucket and configure Cloudfront to point to your bucket
2. Use Route 53 to point a custom DNS domain name to your site
3. Secure your URL using HTTPS
   

STEP 1: Upload your website to an S3 Bucket
---
* Login to the AWS console and navigate to S3.
* Click "create bucket". Enter your bucket name (must be unique). You can leave everything else on the default settings. We are keeping the public access to this bucket as "block all public access". (We will use CloudFront to access the bucket later). Proceed with creating the bucket.
* Click on your bucket and upload your website files. Leave default settings and proceed to upload.
  
![Screenshot (73)](https://github.com/user-attachments/assets/baf43fae-15de-4a0a-b9a5-0063910f8938)
//Remember these are not publicly accessible so if you are trying to reach the site with the url it will say access denied. You can click "open" in the console to access it if you want to make sure it worked.

* Now we want to allow the public to access the bucket so navigate to CloudFront and click "create a cloudfront distribution"

![Screenshot (74)](https://github.com/user-attachments/assets/91d0963b-f0e9-4684-9b59-d1b563001dbe)

* Choose your S3 bucket you just created as the origin domain.
* Choose the recommended "origin access control setting". This gives cloudfront access to the objects in the bucket but nothing else.
* You will need to create an origin access control using the "create OAC" button. You can leave everything default and create.

![Screenshot (75)](https://github.com/user-attachments/assets/27ad1e53-c0b1-4951-8a3e-739ab6015c92)
![Screenshot (76)](https://github.com/user-attachments/assets/9f7d00cb-1618-43b7-9953-34f3f06128b5)

* Scrolling down you do not have to enable the web application firewall right now.
* Scroll down and for the default root object, use your index.html
* Click Create Distribution.
* It will likely tell you that you need to update your S3 bucket policy and will give you a prompt to copy a policy

![Screenshot (77)](https://github.com/user-attachments/assets/14cce2b5-89c1-4c10-90eb-82feb0556b79)

* Navigate back to S3 and go to your bucket and click on "Permissions" scroll down to your "bucket policy and click edit and paste in the policy you copied.

![Screenshot (78)](https://github.com/user-attachments/assets/1198b7ab-21a5-4b1f-b751-98fd2e425255)

* Now you can navigate back to cloudfront and your distribution should be ready. You can copy the "distribution domain name" search it and you should have access!

![Screenshot (80)](https://github.com/user-attachments/assets/7df530de-0b79-4a8b-bd77-bf6f22d561b4)



STEP 2: Create a custom domain with Route 53
---
* Right click on the AWS logo in the upper left corner and select "open link in new tab". Open Route 53
* Navigate to "registered domains" and select "register domains"
  
![Screenshot (71)](https://github.com/user-attachments/assets/207e400f-4891-4aad-b086-9ea9107a7f17)

* Type in the domain namee you want and see if it is available. If it is, it will show you pricing options to obtain that domain. Find the one that is suitable and click next, fill out your information to register the domain and make certain to turn on privacy protection at the bottom! If you submit, just know you will pay for the custom domain. So continue to submit and AWS will review the request and you will have your custom domain shortly!

* Now let's configure some records so that our DNS servers know where to find us.
* Navigate to "hosted zones" and you will see your records.

![Screenshot (72)](https://github.com/user-attachments/assets/5d10db9c-c3d4-4f18-8e1a-cac19267050f)

* Click create record and we will give this the "test" subdomain name and make it an A record. Meaning it holds the site's IPv4 address. Use 11.22.33.44 and create.
* If you type in the IP to your browser, nothing will come up because we do not have the IP yet. But what is happening behind the scenes?
* Click on the "CloudShell" logo in the bottom left of your console and open a new tab.
* Normally you could type in "nslookup" on windows or "dig" for mac but that does not work here because nothing is installed on cloudfront yet.
* Type "sudo yum install -y bind-utils" to install these commands
  
![Screenshot (67)](https://github.com/user-attachments/assets/7e53cd4c-4d7d-4506-bec4-9d3aa3764712)

* Now you can use your commands. Type in "nslookup (your record)" and you should be able to see that your domain name is at the IP 11.22.33.44. You can also type in the "dig" command with your record to see a bit more detail such as your TTL if you want to see that.
  
![Screenshot (69)](https://github.com/user-attachments/assets/a52dcfb2-f16b-45dd-88a7-8dc4de743d54)

So there you have it. We are all set with our first record. Now we will use records to easily navigate to our resume.

Click create record. -> record type = A -> click alias and choose s3 as your endpoint. Select the region your s3 are hosted in.
Your endpoint should show up in the drop down menu. If not, it might be because your endpoint is different then your domain name. 
Create Record.
Now you can look up your domain name and it should navigate to your site!


STEP 3: Secure your site!
Open up a new tab in the console and navigate to certificate manager. -> Certificates -> Request certificate -> Request public certificate.
Type in your domain name from route 53. choose DNS validation. Default everything else and then request.
Next scroll down to "Domains" and select "create records in route 53" and click "Create records". This will create you a new CNAME record in Route 53.
Once your certificate request is successful navigate to cloudfront.
Click on your distribution -> scroll to settings and click "edit". When you get to "alternate domain name" click add and enter your domain name.
Scroll down to custom SSL certificate and choose the certificate you just created and save changes.
If you navigate to your cloudfront distribution domain name it should navigate to your site and you will notice it is now secure and shows the lock.

![Screenshot (83)](https://github.com/user-attachments/assets/83b8c1ce-e7ca-4b40-a177-c72e5baf5d42)

The last thing we need to do is update our A record to go through cloudfront. 
Navigate back to Cloudfront and go to your "A" record. -> edit record -> Change the alias from the s3 website to your cloudfront distribution and save.

There you have it. Your resume is now posted online and is secure using HTTPS!



Last last activity you need to do is make sure that you delete the resources if you do not want to keep your website deployed. 


