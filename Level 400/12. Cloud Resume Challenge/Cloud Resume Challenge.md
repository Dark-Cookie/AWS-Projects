
![](https://cdn-images-1.medium.com/max/3840/1*UoBV4LwdSgIei2DVbck5pA.gif)

## AWS Project: The Cloud Resume Challenge

My story of the AWS Cloud Resume Challenge

## About Me

Hi, I am Asif Khan, and I am 24 years old. I live in India and have a bachelor’s and a master’s degree in Computer Science, graduating in 2024. My previous job involved blogging about technical topics like Data Science and Software Development, which then transitioned into creating AI courses for Udemy. As for cloud experience, I have none. However, I got my first computer when I was 4 years old (in 2004), so I have been familiar with computers for a long time. When I was 16 (in 2016), I decided that I wanted to do something in cloud, but then college started, and I couldn’t pursue it until this year.

In April, I came across the Cloud Resume Challenge and had no idea what the cloud was or why it was booming or what is AWS. However, at the time of writing this blog, I have completed over 30 AWS projects, including the CRC, and I have also earned two AWS certifications: AWS Certified Cloud Practitioner and AWS Certified Solutions Architect — Associate.

### Why I Decided to Do the Cloud Resume Challenge

I graduated this year in April, and since I was 16, I’ve wanted to work in the cloud, so I felt it was the right time for me to start. While watching YouTube, I came across a YouTuber named [Soleyman](https://www.youtube.com/@techwithsoleyman), and that’s when I first heard of “[The Cloud Resume Challenge](https://cloudresumechallenge.dev/).” Even though it was my first time hearing about it, I couldn’t forget the name after that.

I made a list of things I wanted to do, such as taking online courses, exploring the AWS Console to get familiar with AWS and its services, and completing [**Andrew Brown’s **Certified Cloud Practitioner (CCP) course](https://youtu.be/NhDYbskXRgc?si=8XHp7YFofS1-4_FG). It was a 14-hour course, and after watching nearly 8 hours of it, I encountered something known as the “**forgetting curve**.” This concept describes how information is lost over time if there’s no active attempt to retain it.

I realized that simply watching theory would not be enough, so I began searching for beginner-friendly AWS projects to help me navigate the AWS Console. Honestly, many of the projects I found seemed too easy, even though I hadn’t touched the AWS Console yet. Then I remembered the **AWS Cloud Resume Challenge**, and without hesitation, I decided to start it as my first-ever AWS project.

### The Cloud Resume Challenge as my first AWS Project

Was it a mistake to start with the Cloud Resume Challenge as my first AWS project, given that I had zero knowledge of the AWS Console and its services? Well, here’s what I learned:

**The Downside:**

* It took me a month to complete the Cloud Resume Challenge.

* I had no idea what an API, Infrastructure as Code, or tests were, so I had to search and learn about them on the internet before diving in.

* Although I completed the CRC, I had no real understanding of AWS services beyond those used in the challenge itself.

**The Bright Side:**

* While studying for the AWS Certified Cloud Practitioner exam, I learned about many AWS services such as AWS Organizations, DynamoDB, API Gateway, and more. By the time I had already completed the CRC, the theory from the video courses helped solidify my knowledge. That combination of hands-on and theoretical learning ultimately helped me pass the AWS SAA-03 exam.

* I learned several AWS best practices along the way.

* I gained experience with website hosting, DNS, HTTP vs HTTPS, GitHub CLI & Actions, Infrastructure as Code (Terraform and CloudFront), and much more. All of this helped me pass the SAA-03 exam.

It felt like playing at a high level from the start, and that challenge motivated me to take on more projects like “Serverless Data Processing on AWS,” “Large-scale Data Processing with Step Functions,” and “Web Applications based on Amazon EKS.” You can check my [**LinkedIn](https://www.linkedin.com/in/asif0108/)** profile for more.

Looking back, from when I started the CRC to today, with 2 AWS certifications and 30+ AWS projects, I can say it was totally worth it. Thank you, [**Forrest Brazeal](https://forrestbrazeal.com/)**, for creating this amazing challenge. I truly don’t know where I’d be if the CRC had not been my first AWS project.

### What after the Cloud Resume Challenge?

With the CRC completed, two AWS certifications earned, and 30+ AWS projects created, it’s now time for me to find a job in the cloud. By the time I start my job search, you can read below about how I completed the CRC. Also here are my [**portfolio](https://asif-khan.click/), [LinkedIn](https://www.linkedin.com/in/asif0108/), and [GitHub](https://github.com/Dark-Cookie).**

### Architecture Diagram

![Architecture Diagram of The Cloud Resume Challenge](https://cdn-images-1.medium.com/max/3840/1*UoBV4LwdSgIei2DVbck5pA.gif)

### Chunk 0: Certification prep

Before starting the CRC, I didn’t have any AWS certifications, and by the time I completed the CRC, I still didn’t have any. In fact, I passed the CCP in October 2024 and the SAA-03 in November 2024. So, it’s just like Forrest Brazeal said: you can do any chunk in any order.

### Chunk 1: Building the front end

**HTML and CSS**

For the HTML & CSS part, it wasn’t an issue for me, as we were taught that in college as part of the syllabus. Although, it’s worth noting that I didn’t create a portfolio from scratch; instead, I used some of the templates that were available online.

**DNS and CDN**

Before CRC, I knew DNS only by its full form — that was it, nothing else. But while working on the DNS section, I had the opportunity to learn more about DNS, Route 53, and Content Delivery Networks. With this clearer understanding, I was able to figure out the tasks as described in the CRC. The knowledge I gained about DNS and CDNs was definitely valuable. Thanks, **Forrest!**
>  **Figuring out this part motivated me to believe I could complete the next section as well**

### Chunk 2: Building the API

For this chunk, setting up a DynamoDB table was easy, and setting up an API Gateway was also straightforward. However, connecting them using Python took me some time. I learned more about APIs and Python, and eventually, I completed this part within a few days. Now, reflecting on it, I realize how important APIs are in the cloud, and thanks to CRC, I can confidently say I learned about APIs through this experience.

As for GitHub, we were taught GitHub in college, but I had forgotten how to use it. I did manage to figure it out, but I used GitHub Desktop. At that time, I was into Linux OSes and wanted to work with GitHub through the command line, which motivated me to try GitHub CLI. I then set up source control, which I used within Visual Studio Code. And there it was — my CI/CD setup, aka source control.

### Chunk 3: Front-end / back-end integration

After creating both the front-end and back-end, it was time to integrate them. The first task was to create a JavaScript counter that would increment every time a user visited our portfolio website. This was easy to implement, taking only a few minutes, but figuring out the logic behind how things would work took me more time.

After setting up the visit counter, it was time for the **‘TESTS’**. Before CRC, I had no idea what tests were, why they were necessary, or how to perform them. Luckily, the [**Guidebook](https://forrestbrazeal.gumroad.com/l/cloud-resume-challenge-book)** provided some guidance on how to run tests. With that, I experimented with both [**Cypress](https://www.cypress.io/)** and [**Playwright](https://playwright.dev/)**, learning how testing works. I ran both unit tests and end-to-end tests, but during the process, I misplaced my code for the end-to-end tests.

Lastly, **CORS** was a PITA! A few days ago, I worked on a project that involved API testing, and I tried using [**Postman](https://www.postman.com/)** for it. Fortunately, I was already familiar with testing, so it wasn’t too bad. 😄

### Chunk 4: Automation / CI

Since the beginning of the CRC, I was excited about automation, specifically Infrastructure as Code, and the idea of using Terraform sounded really interesting. So, when I reached this part, I had to go through some Terraform documentation to figure out how to get it running with AWS. At the same time, I learned how to use the AWS CLI, including managing Access Keys and Secret Keys. Terraform took me about a week or two, but eventually, I was able to create a Terraform script that would provision most, if not all, of the components needed for the CRC.

Later, in my projects, I also came across an AWS service called CloudFormation, which is another template-based IaC solution. So now, I have two IaC tools under my belt.

### Chunk 5: The blog … and beyond

You’re currently reading Chunk 5, so yes, this is where my journey through the Cloud Resume Challenge (CRC) ends.

For someone new to AWS and the cloud, remember that hands-on practice is key to learning. When combined with theory, it’s a powerful combination. If you’re new to performing the CRC, here’s my advice:

 1. You might feel overwhelmed at times, but don’t worry. Stick with it, and in the end, you’ll get through it and complete the challenge.

In the blog above, I haven’t explicitly mentioned how I performed certain tasks because I believe that working through them yourself is the best way to learn and understand. It will pay off in the long run.

Lastly, thank you again, **Forrest Brazeal**, for creating this challenge. It has helped me in so many ways that if I started writing them all down here, it would get very ! 😅 Good luck to anyone new to the cloud or starting the Cloud Resume Challenge. You’ve got this!
>  ***Asif Khan — Aspiring Cloud Architect | Weekly Cloud Learning Chronicler***

[**LinkedIn](https://www.linkedin.com/in/asif0108/)**/[**Twitter](https://x.com/asif26073)**/[**GitHub](https://github.com/Dark-Cookie)**

## Links

* Click [here](https://asif-khan.click/) to view the website/portfolio developed
