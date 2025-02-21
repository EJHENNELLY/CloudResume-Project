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


