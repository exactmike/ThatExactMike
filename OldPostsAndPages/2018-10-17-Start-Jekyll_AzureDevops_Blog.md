---
title: "Start-Jekyll_AzureDevOps_Blog"
excerpt: "Blogging with Jekyll, Azure DevOps, Github, Azure Storage and Cloudflare."
date: 2018-10-17
last_modified_at: 2019-01-10
comments: true
tags:
  - blog
  - jekyll
  - Azure
  - devops
  - github
  - ubuntu
  - vscode
  - minimal-mistakes
---

## background

I used the design of this blog and it's workflow to help me learn more about devops pipelines and to specifically explore features of the recently re-branded Visual Studio Team Services (now known as Azure devops).  I also dove into some (new to me) technologies for generating a static website.  Here's the stack of technology that I am currently using to design, publish, and maintain this site:

## blog technology stack

- on x # of workstations:
  - Windows 10 with Ubuntu app installed
  - Jekyll on Ubuntu on Windows 10 (static site generator)
  - VcXsrv on Ubuntu on Windows 10 (x-Server to support native vscode)
  - VSCode on ubuntu on Windows 10
  - Git repository for the local copy of the blog (clone of the github repo below)

- in the 'cloud'
  - Git repository on github connected to Azure DevOps (origin for any local copies)
  - Azure Devops Build Pipeline that builds the site using Jekyll
  - Azure Devops Release Pipeline that publishes the built site to Azure Storage
  - Azure Storage static website hosting
  - Cloudflare CDN for DNS, SSL, etc.

- other tools
  - [Minimal Mistakes template](https://mmistakes.github.io/minimal-mistakes/)

## initial setup process

Getting up and running with ubuntu, ruby, jekyll and git was pretty easy (what I'll call the baseline dev environment here).  It literally took just a few minutes to get to the point of locally running a jekyll site.  Being new to how jekyll templates work, the tricky part was getting my chosen template, Minimal Mistakes 'installed'.

### getting the baseline dev environment up and running

#### from Windows

```powershell

Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
Invoke-WebRequest -Uri https://aka.ms/wsl-ubuntu-1804 -OutFile Ubuntu.appx -UseBasicParsing

```

#### from Ubuntu

```bash

# update/upgrade unbuntu
sudo apt update && sudo apt upgrade
# add ruby
sudo apt-get install ruby ruby-dev build-essential
# remove unneeded
sudo apt-get autoremove
# ruby environment setup
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME=$HOME/gems' >> ~/.bashrc
echo 'export PATH=$HOME/gems/bin:$PATH' >> ~/.bashrc
# make the environment settings above effective
source ~/.bashrc
# configure/install gems
gem update
gem install jekyll bundler
sudo apt-get install zlib1g-dev #found this was needed by jekyll or bundler
bundle config path vendor/bundle
# create a directory to contain the new site
jekyll new .#create the 'base' jekyll site - see below for further information
git init
git add *
git commit -m 'initial commit'
# set up your repo on Github and then add it as remote origin

```

Note: To contain the site files I used a directory under my Windows user profile ($env:USERPROFILE).  However, I use ubuntu vscode and git to edit the contents.  I will drop new files in there via Windows but avoid editing them from the Windows side after they are in there. There are probably other ways to handle the potential conflicts but this is what I've settled on for now.
{: .notice--info}

### getting linux vscode working on ubuntu/windows

I followed this [gist](https://gist.github.com/fedme/604c6b811939468fdad06e3fbba942ed) to get an xserver running and to install/run vs code.

### getting Minimal-Mistakes installed

I'm admittedly a complete newb with regard to jekyll.  I didn't want to fork the minimal-mistakes repo since there was an option to install the template as a gem. However, I found that installing via gem method did not really give me a complete site.  I still had to either previously generate or later copy some files and folders from the minimal-mistakes repo or from other minimal-mistakes users ([Thanks, Dave!](https://powershell.anovelidea.org/))

To install the template as a gem I did the following:

```bash

jekyll new .

```

This results in the following file/directory structure:

```powershell

Get-ChildItem | Select Name,PSIsContainer

Name         PSIsContainer
----         -------------
vendor                True
_posts                True
.gitignore           False
404.html             False
about.md             False
Gemfile              False
Gemfile.lock         False
index.md             False
_config.yml          False

```

From there I followed the [instructions](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/) to install minimal-mistakes as a ruby gem.  However, I also cloned the [minimal-mistakes repo](https://github.com/mmistakes/minimal-mistakes) to another location and copied _config.yml from there.  Eventually, I also copied additional files/folder structure over from other minimal-mistakes based sites.  Most significantly I created _pages and archive and under _data I created a naigation.yml file to control the overall site navigation.

## the release pipeline

using Github integration with Azure Devops Pipelines (Build and Release) ... more detail soon. However, in the meantime, [this series of posts](https://www.forevolve.com/en/articles/2018/07/10/how-to-deploy-and-host-a-jekyll-website-in-azure-blob-storage-using-a-vsts-continuous-deployment-pipeline-part-4/) got me started.

## ongoing workflow

... more soon