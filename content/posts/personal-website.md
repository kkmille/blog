---
title: "Getting my website up and running"
date: 2022-07-28
draft: false
---

## Tasks
1. Find and register a good domain name
2. Choose the right framework
3. Create the skeleton and a few first posts
4. GitHub
5. Deploy the website on AWS

## Choosing a domain name
I wanted to find a domain name that's both self-explanatory and easy to remember. My target was to have both my name and cloud in the URL and not pay to much for it (max 10-20$ / year). I came up with two valid options:
- select a TLD like `.org` (e.g., `camilleinthecloud.org`)
- select `.cloud` TLD (e.g., `camilleinthe.cloud`)

I thought the price difference was acceptable (12$ for a `.org` and 25$ for a `.cloud`) and I also thought a `.cloud` domain name was more interesting so I ended up selecting `camilleinthe.cloud`

## Registering a domain name and setting up a public hosted zone in Route 53
You could use another registrar to register a domain name but it is simpler to use AWS for both the name and the zones - it is also a good way to learn how to do it.

### Registration
Registering the name is as simple as navigating to the Route 53 page, selecting a domain, adding it to cart, filling in a few administrative information and paying for it.

The registration may take a while to complete and you may need to verify the domain using the email address you set up earlier. Mine took about 10 mins all in all.

### Hosted zones
When you register a domain with Route 52, AWS automatically creates a public hosted zones with the following records
- An `NS` record poiting to the various AWS DNS servers
- An `SOA` record poiting to the AWS root DNS server

## Choosing a framework to host a static website
I wanted something simple yet powerful and customizable. I came across the following in my research:
- Hugo
- Wordpress
- Other
- Other
- Other

I ended up selecting Hugo for its simplicity, performance and numerous themes/plugins but to be honest given what I was going to do with it I could have chosen any of them and it would have done the job.

## Skeleton
RTFM

## Deploying the website on AWS
I wanted to take this opportunity to experiment with various AWS services so I decided to try deploying the website on multiple compute platforms meant for static websites, namely:
- `Amazon S3` using Hugo Deploy (https://gohugo.io/hosting-and-deployment/hugo-deploy/)
    - Follow hugo deploy
    - Set up a CloudFront distribution (with the proper security features such as ACM, WAF and OAI) and a custom domain name
    - Set up CD pipeline (CodeDeploy hooked to a Git web hook?)
- `AWS Amplify` (https://gohugo.io/hosting-and-deployment/hosting-on-aws-amplify/)
