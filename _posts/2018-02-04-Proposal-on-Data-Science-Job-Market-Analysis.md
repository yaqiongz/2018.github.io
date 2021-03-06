---
layout: post
title: Proposal on Data Science Job Market Analysis
---


## Data science job market analysis and job recommendations
Author: Yaqiong Zhang

Date: Feb. 3rd, 2018

### Introduction:

Data science become so popular nowadays, and data scientist was marked as the best job in the USA from Glassdoor, 3 years in a row. 

When I learn data science, I find it's really interesting. I gain all kinds of knowledge and skills, such as programming, machine learning, statistics, database, big data and even story-telling. And when I look at the job descriptions of data scientist, I found that different companies emphasis different skills in their job descriptions. Moreover, since data science is a fast growing field, there are new skills required in a data scientist job. 

Therefore, it's valuable to have an analysis on the data science job market for those job seekers like me. 

### The questions will be answered
1. What's the top 10 most mentioned skills? Can you visualize the them in a USA map with those major cities?
2. Can you classify a job description? Or in another word, can you put some tags in a job description? 
3. Generate a questionnaire and a decision tree for someone to answer the questions and give a recommend a job?

### How?
1. I will use a web scraper to scraper job search website such as linkedin, glassdoor or indeed. The target is to get at 1,000 of job descriptions in a database. Ideally, I want to use a free API. But it seems that those job search websites are careful about their data.

2. Count the number of how many times of the data science skills were mentioned in those job descriptions I got.

3. Get some visualization in a USA map. I can map the number of data science jobs in those major cities. I can map the popular skills required in those jobs in the major cities. 

4. Then, organize the job descriptions. **I'm not sure what is the best way.** I may want to come up some tags, for example, I can ask is this job emphasis research or this this job emphasis business intelligence. And I may want to build a model to give job descriptions with tags. 

5. With all the job descriptions in the database organized, I want to build a recommendation system. The job seeker will be asked by a few questions and the system will come up with a few jobs in the database so that the background of the job seeker matches. Just like [akinator](http://en.akinator.com/), building a interactive decision tree in a web app will be very attractive. 


#### resources of the web scrapers
1. [Glassdoor Web-Scraper](https://github.com/sallamander/web-scrapers/tree/master/glassdoor)
2. [Landing my dream job by scraping Glassdoor.com](https://nycdatascience.com/blog/student-works/web-scraping/glassdoor-web-scraping/)
3. [Web Scraping Job Postings from Indeed](https://medium.com/@msalmon00/web-scraping-job-postings-from-indeed-96bd588dcb4b)
4. [Scrape Data Scientist's Skills from Indeed.com](https://github.com/steve-liang/DSJobSkill)

