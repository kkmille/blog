---
title: "Getting my website up and running"
date: 2022-07-28
draft: false
---
**Table of Contents**
- [Choosing a domain name](#choosing-a-domain-name)
- [Registering a domain name and setting up a public hosted zone in Route 53](#registering-a-domain-name-and-setting-up-a-public-hosted-zone-in-route-53)
  - [Registration](#registration)
  - [Hosted zones](#hosted-zones)
- [Choosing a framework to host a static website](#choosing-a-framework-to-host-a-static-website)
- [Installing and configuring Hugo](#installing-and-configuring-hugo)
- [Deploying the website on AWS](#deploying-the-website-on-aws)
  - [Using S3 and CloudFront](#using-s3-and-cloudfront)
  - [Using Amplify](#using-amplify)

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

We'll be using CloudFront to serve the website so we'll need to add the relevant records when the time comes.

## Choosing a framework to host a static website
I wanted something simple yet powerful and customizable. I came across the following in my research:
- Hugo
- Wordpress
- Other
- Other
- Other

I ended up selecting Hugo for its simplicity, performance and numerous themes/plugins but to be honest given what I was going to do with it I could have chosen any of them and it would have done the job.

## Installing and configuring Hugo

There is no dirth of end-to-end guides to do so hence I will not include any detailed information.

I personally used the following:
- Hugo: https://gohugo.io/getting-started/installing/
- Congo theme: https://jpanther.github.io/congo/docs/

## Deploying the website on AWS
I wanted to take this opportunity to experiment with various AWS services so I decided to try deploying the website on multiple compute platforms meant for static websites, namely:
- `Amazon S3` along with `Amazon CloudFront`
- `AWS Amplify`

### Using S3 and CloudFront

Deploying the website turned out to be a little more complicated than I thought, but it's still relatively simple. Hereunder the required steps:
- Set up `AWS CodePipeline` and `AWS CodeBuild` to
  - Connect to the git repo
  - Start a new deployment when a code change happens
  - Build the static pages from the code (example of buildspec file below)
  - Deploy the new version of the static files to the S3 bucket
- Invalidate the cache on new deployment to avoid stale posts (without having to lower the TTL to a useless level - you still want most assets to be cached for as long as possible)
- Configure the `Amazon CloudFront` distribution
  - Tasks 1
  - Tasks 2

The buildspec.yml file I used
```toml
version: 0.2
 
phases:
  install:
    runtime-versions:
      python: 3.10
    commands:
      - apt-get update
      - echo Installing hugo
      - curl -L -o hugo.deb https://github.com/gohugoio/hugo/releases/download/v0.101.0/hugo_0.101.0_Linux-64bit.deb
      - dpkg -i hugo.deb
  pre_build:
    commands:
      - echo In pre_build phase..
      - echo Current directory is $CODEBUILD_SRC_DIR
      - ls -la
  build:
    commands:
      - hugo -v
artifacts:
  files:
    - '**/*'
  base-directory: public
```

Here is a simple diagram detailing the architecture and deployment pipeline
![Architecture diagram](/camille-cloud-blog.png)

Sources:
- Source 1
- Source 2

### Using Amplify

