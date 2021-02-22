---
layout: post
title:      "React - Vintage Camper Restore"
date:       2021-02-22 02:01:13 +0000
permalink:  react_-_vintage_camper_restore
---


### The plan
For this react project, I wanted to build something that would be useful to me, would put into practice what I've learned and be fun to build. With that in mind, I created Vintage Camper Restore, an app that allows me to keep track of the camper projects I'm working on. I only have one trailer I'm working on, but the goal of this app was to make it easier for me to keep track of inventory and the supplies I need. If I ever expanded on the app, I'd add accounts so other folks could create and track their own projects.

### Rails API and building the scoffold 
Once I had create-react-app going, I started out building the rails API. That part felt really familer. I built a lot of the same functions I had in my JS project, which made the process pretty smooth. I knew I would need models for projects, inventory and supplies. I also decided to make my backend and frontend into separate repos. There's additional set up and you're commiting/pushing code to two different places, but I think those extra steps are worth the time for a few reasons 1) it makes navigating the project much easier, 2) my branches and commits are specific to either the frontend or backend and 3) if something goes wrong and I need to revert, I'm only doing so in one place with one set of code that all goes together. I would come back to the backend throughout the project to make small tweaks, but it was mostly set up before I started writing any React. 

### Component building
I found [these walkthrough videos](https://instruction.learn.co/student/video_lectures#/?query=expense-tracker) really helpful for getting started. I spent a lot of time in the console, throwing debuggers in my code to figure out issues. I highly recommend installing both the Redux and React dev tools for whatever browser you're using. Most of the challenges (and therefore time spent) I had were in passing props between components and then extrapolating them to get to the right element. This is definitely where the dev tools came in handy. As mentioned above, I revisited the backend to fix things along the way. One area I got stuck on was looking at a particular project, I wanted to show an inventory list. The expectation I had was that each item in the inventory would show up through the index. This wasn't happening. I was only getting the first item in my seed data. When I navigated to the API, I was seeing the full list. After some time in pry, I realized I set up the Project>Inventory relationship as `has_one`. I read that relationship too literally - yes a project will have one inventory, but there will be many items in that inventory. Changing to association to has_many fixed that issue.

In my Project component, I defined project as `let project = props.projects[props.match.params.id - 1]` which is working currently. I'm curious to see if that will hold up because the id’s of my projects might not always line up with the index of the array. 

I'm sure there's a good bit of refactoring that I can do, but I feel good about this MVP.

