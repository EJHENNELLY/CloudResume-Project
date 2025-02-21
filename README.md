# CloudResume-Project
Deploying my resume with AWS

I have been working to transition into a tech career for some time now and like most people, I spent tons of time, money, and effort stressing over certifications that I was led to believe would be the key to me landing a role in the dynamic tech industry. After having achieved the certifications that I thought would change everything, I started applying to jobs and began realizing that while these certs showed I was a quick-learner and were "proof" that I had the necessary knowledge to do the job, I still did not have much to offer in terms of anything that I had built. My cybersecurity and security engineer certifications mostly just allowed me to show screen shots of walk throughs that I had done during the course of my learning. So, I decided to create something to show to hiring managers. But what? I hate to admit that it took me sometime to determine what to create. In the end, it seemed fairly obvious.... A resume.

In this project, I am going to walk through the steps I took to create this resume and the things I learned along the way.

I will use a number of programming languages and AWS services in this project:
Programming = HTML, CSS, Javascript, Python,
AWS Services = S3, Route 53, CloudFront, DynamoDB, API-Gateway, Lambda 

First things first, you need a few things to get started:
- An AWS account with access to the console
- Basic understanding of code
- A resume that you can deploy (needs to be html with some css) so you have something nice to show off when hiring managers visit

If you do not have a resume ready to deploy, I suggest searching on youtube or medium for instruction/inspiration! It can be very basic at first and you can update it later.

Once you have your basic resume ready to deploy we are going to follow these steps:
1. Upload your resume to AWS S3 bucket to host as a static website
2. Use Route 53 to point a custom DNS domain name to your site
3. Secure your URL using HTTPS by utilizing CloudFront
4. Create a Visitor counter so you can see how many people access the site (will be helpful while networking on job hunt)
5. Store the visitor data
6. Create a way to allow your site and database to communicate with each other
7. Test everything to make sure it works

STEP 1:
---



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

So there you have it. We are all set with our first record.
