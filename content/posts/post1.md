---
title: "How to blog CI/CD style 2020"
date: "2017-08-21"
description: "How to blog CI/CD style 2020"

# Theme-Defined params
thumbnail: "img/post1Image.webp" # Thumbnail image
lead: "Set-up blog the easy way"
---



Building website using Hugo, GitHub and Netlify 





I have recently decided to start a blog and i looked at a bunch of different content management systems like WordPress , github pages etc. but it didnt quite feel like what i wanted . I'm a solo author and i wanted to put my content out there with leat amount of effort where i can focus on the content and not worry about the technology part. So, I finally settled down for the most elegant solution(in my opinion) which is Hugo with Netlify
Following is the workflow if you want a look at how i set up my blog

![Workflow](/img/workflow.png)

In every CI/CD pipeline there are multiple parts 

1. Source code 

     Every content management system starts with source code and i used > Hugo which is a static site generator swritten in Go. There are other alternatives like - . I used Hugo because its open source and  websites built with Hugo are blazing fast and secure . Websites are rebuilt whenever change is made.

2. Storage 
    
    We need to store files somewhere , in my case storage part is taken care by github. Other storage options like gitlab can be used too. 

3. Build and Deploy part 
    
    Site can be deployed using modern hosting services like



https with letsEncrypt (netlify takes care of it woth just one click of it)


Will explain with Hugo Mainroad theme


