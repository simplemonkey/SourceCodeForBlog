---
title: "How to blog CI/CD style 2020"
date: "2020-02-10"
description: "How to blog CI/CD style 2020"

# Theme-Defined params
thumbnail: "img/post1Image.webp" # Thumbnail image
lead: "Set-up blog the easy way"
---


Building website using Hugo, GitHub and Netlify 

I have recently decided to start a blog and I looked at a bunch of different content management systems like WordPress , github pages etc , but it didnt quite feel like what I wanted . I'm a solo author and I wanted to put my content out there with leat amount of effort where I can focus on the content and not worry about the technology part. So, I finally settled down for the most elegant solution(in my opinion) which is Hugo with Netlify
Following is the workflow if you want a look at how I set up my blog

![](/img/workflow.png)

In every CI/CD pipeline there are multiple parts 

1. Source code  

     Every content management system starts with source code and i used > Hugo which is a static site generator written in Go. There are other alternatives like Jekyll, Hexo . I used Hugo because its open source and  websites built with Hugo are blazing fast (< 1 ms per page) and secure . 
Also you can experiment on local machine.

2. Storage 
    
    We need to store files somewhere , in my case storage part is taken care by github. Other storage options like gitlab can be used. 

3. Build and Deploy part 
    
    Netlify is the go-to hosting service if you want to  build, deploy, host and manage HTTPS (with letsEncrypt) all under one roof. 
    


Below are the steps to set up a blog with mainroad theme , just like I did

Prerequisites:
 
> Install latest version of hugo from here(if you want to run the server locally and test before deploying) :  https://gohugo.io/getting-started/installing/ 
> Github account ,Gitbash
> Netlify account

Step one : Hugo Setup

input  : nothing
output : Site in hugo format with mainroad theme which is not built.
    Note : The reason its not built here is because we have to manually build and push every time a change is made. We are implemeting CI/CD pipeline and thats where Netlify takes care of the rebuilds everytime a change is made in the source code. 

Create a new Site 
``` 
$ hugo new site site_name
 ```

Initilize git in the root
``` 
$ git init
```

Select & Download a  theme
```
$ git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/mainroad
```
Add theme in config.toml in the root

theme = "mainroad"

Adding Posts

Create content using markdown files in the content folder of the root with front matter.

Adding Images 

All the images are in static folder . Create a img/ folder and place all the images required for the site in here.

To test site locally , run  
```
hugo server -D 
 ```

>> Insert screenshot 

The Site should be available at localhost:1313/ if the port is free

Step 2 : Github Setup 

Input  : Hugo Source Code
Output : Github repo

Follow guidelines from github help pages to push your local project to github repository : https://help.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line


Step 3 : Build and Host on Netlify

Conect your Netlify account with Github and authorize application with Github

Select new site from Git and select the repo you want to host
>>Screenshot

In the Netlify console, selecting “Deploy site” will immediately take you to a terminal for your build. Once the build and deploy is finished , select Browse Site to view the live site
>>Screenshot

Adding Domain name 

Adding HTTPS certificate 
