---
layout: post
title:      "Garden Builder - a Rails app for planting a garden"
date:       2020-09-09 01:43:44 +0000
permalink:  garden_builder_-_a_rails_app_for_planting_a_garden
---


### The plan

Going into the rails project, I was initially thinking I'd create an app for social workers to use to track their interactions with clients. After spending a little bit of time sketching out what that might look like, I decided to scrap the idea and build off of my sinatra project - Plant Tracker. That's how I came to create Garden Builder - an app that allows users to create gardens, plant plants in them and track tasks for their gardens. 

### Setting up models, associations and routes

As the project outline mentions, setting up models and how they relate was definitely the trickiest part. I found it really helpful to sketch out my app on a dry erase board. This made it easy to move things around and present my idea outloud to someone else (which always helps me think through the logic behind my set up).

I initially had a column name of "type" on my garden model. In the midst of creating a garden, I had to change that column name due to a SQL error - type seems to be a reserved word.

Building out the routes, I ran into an issue where the gardener and garden ids were being swapped in the URL. I went back to read this:  https://learn.co/tracks/full-stack-web-development-v8/module-13-rails/section-10-routes-and-resources/routing-and-nested-resources. Ended up with this route `gardener_garden_path(g.gardener.id, g.id)` in a do block which gave me this (correct) URL structure: http://localhost:3000/gardeners/4/gardens/15

Once I had the gardener and garden structure set up and working properly, it was fairly easy to duplicate that logic so it applied to my plant model (which needed to be associated to a garden). 


Mid-way through the project, I decided to add a Tasks model and table so a gardener could create tasks for a specific garden. They'd also be able to check them off when the task was complete. Adding the table and functionality in at this point was a little tricky. After I added the tasks table, I realized I needed to add the garden_id foreign key, which I did by following this answer: https://stackoverflow.com/questions/22815009/add-a-reference-column-migration-in-rails-4


### Omniauth and Github login

This requirement felt a little daunting at first, but luckily there were some great examples I could follow. I used the figaro gem to set up environment variables - http://railsapps.github.io/rails-environment-variables.html
 
In my find_or_create_by_omniauth method, ``Auth_hash["info"]["email"]``  looked like I was being logged in, but I was logged in as the first Gardener (since that account had a nil email). When really I should have been logged in as gardener_id = 10 (the account I made with my email which is linked to Github)
When I tried to log in as id = 10, the above happened. Looking at request.env["omniauth.auth"] in the console, the email field was nil so Github wasn’t returning (or at least not showing) my email.

To troubleshoot, I changed my find_or_create_by_omniauth method to:
    ```self.where(:username => auth_hash["extra"]["raw_info"]["login"]).first_or_create do |gardener|```
because in the Github request, I could see my login username for Github. I wanted to know if this would work because if it did, I’d know that my method was working and my sessions#controller was also working.
It did work; I was being logged in as the right gardener.

After lots of googling, I couldn't figure out why my email was returning nil from Github’s request, so I decided to leave username in my method.

### Never done?

I keep a running list of to-dos and try to stick to them as I code. There seems to always be something I'd like to improve or change. I'll likely come back to this app to make those improvements, but for now I'm happy with the way it's working.





