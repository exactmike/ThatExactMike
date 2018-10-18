---
title: "Start-Jekyll_AzureDevOps_Blog"
excerpt: "Blogging with Jekyll, Azure DevOps, Github, Azure Storage and Cloudflare."
date: 2018-10-17
last_modified_at: 2018-10-18
comments: false
tags:
  - blog
  - jekyll
  - Azure
  - devops
  - github
  - ubuntu
  - vscode
---

# background

I used the design of this blog and it's workflow to help me learn more about devops pipelines and to specifically explore features of the recently re-branded Visual Studio Team Services (now known as Azure devops).  I also dove into some (new to me) technologies for generating a static website.  Here's the stack of technology that I am currently using to design, publish, and maintain this site:

## blog technology stack

- Windows 10 with Ubuntu app installed
- Jekyll on Ubuntu on Windows 10 (static site generator)
- VcXsrv on Ubuntu on Windows 10 (x-Server to support native vscode)
- VSCode on ubuntu on Windows 10
- Git repository for the local copy of the blog (clone of the github repo below)
- Git repository connected to Azure DevOps (origin for any local copies)
- Azure Devops Build Pipeline that builds the site using Jekyll
- Azure Devops Release Pipeline that publishes the built site to Azure Storage
- Azure Storage static website hosting
- Cloudflare CDN for DNS, SSL, etc.

## initial setup process

... coming soon

## ongoing workflow

... coming soon