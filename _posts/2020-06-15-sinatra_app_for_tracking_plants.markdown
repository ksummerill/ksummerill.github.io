---
layout: post
title:      "Sinatra app for tracking plants"
date:       2020-06-15 16:33:22 +0000
permalink:  sinatra_app_for_tracking_plants
---


### Planning ahead and sketching out my app

Before I actually got to the project, I peeked at what the requirements would be so I could spend some time thinking through what I might want to build. Knowing it would be a MVC Sinatra app with ActiveRecord, I started thinking about a practical program that I could implement that would be useful to me. I’ve started gardening in the last couple years, so I came up with the idea to build an app that would allow a user to create an account that they could add plants to, along with information about that plant’s care. I started by writing out my user stories and sketching out the relationships:

**User Stories**
* As a user, I can sign up for an account. 
* As a user, I can log in to my account. 
* As a user, I can log out of my account. 
* As a user, I can create a new plant with name, amount of sunlight and water needed.
* As a user, I can edit any of the features of an existing plant.
* As a user, I can see all my plants on my homepage.
* As a user, I can delete a plant.

Plants would belong to a Gardener and a Gardener could have many plants. I was tempted to make things more elaborate, but knew that could get out of hand fairly quickly so I decided to stick with the above user stories. I can always come back to the app later on to build onto it.

### Coding
I used Corneal for the scaffolding of my app. This made starting out really simple! I started coding from the backend - building out my migrations, models and associations first. I moved on to building out my controllers and getting basic views set up first (index, login and signup pages). I had all my controllers scripted out in application_controller. That file became a bit overloaded, so I moved certain controllers to their own files, splitting out plants_controller and gardner_controller. When I did this, I could no longer render the signup form. After a little investigation, I realized I needed to add “use GardenersController” and “use PlantsController” to my config.ru.

When I got the create routes connected and working (or so I thought) I wanted to be able to click on a single plant and render that plant’s show page (to display information about that plant). I was successfully rendering the show page, but it wasn’t pulling up the right information. I added a binding.pry to the route and realized my params weren’t right. I was getting this:
 
```
 #<Plant:0x00007f9b5e8652f8
 id: 7,
 name: "sage",
 amount_of_sun: nil,
 water_frequency: nil,
 gardener_id: nil>
```

I went back to where I was creating plants to see what params were being set. Nothing except id and name were getting set. So in pry, I executed “Plant.create(params)” and got this error:
ActiveRecord::UnknownAttributeError: unknown attribute 'sun' for Plant.

That led me to my show.erb where I had to change the name=“” from “sun” to “amount_of_sun”. I updated the same thing for water_frequency.

```
<input type="radio" id="shade" name="amount_of_sun" value="shade">
<input type="radio" id="daily" name="water_frequency" value="daily">
```

I created a new plant in the form and hit my pry again. I executed this in the terminal - 
Plant.create(name: params[:name], amount_of_sun: params[:amount_of_sun], water_frequency: params[:water_frequency], gardener_id: session[:gardener_id])

And got this:

```
#<Plant:0x00007f9b5f0fb2c8
 id: 11,
 name: "red pepper",
 amount_of_sun: "partial_sun",
 water_frequency: "once_week",
 gardener_id: nil>
```

Amount_of_sun and water_frequency were being captured. Hooray! Now I had to fix the gardener_id being nil. Changed my code to: gardener_id: session[:user_id] and running Plant.create again gave me a gardener_id.

Another hurdle I faced was when a new account was created, the my_plants homepage was showing every plant ever made by all users. This issue stumped me, so I wrote out the issue like I would file a bug ticket.

Expected: New account → signup → homepage with no plants → button to create a plant
Actual: New account → signup → homepage with all plants ever made → button to create a plant

There were two things I needed to be certain of:
1. Associate new plants with that signed in user
2. Appropriately show plants that only that user had created

I also wrote out each route where a creation or association was being made. For example, post /signup is where I create a Gardener and post /plants is where I create a plant. I knew I needed to comb through each one to determine where the error was. After lots of poking around in pry, I realized I needed to rethink my strategy and grab the Gardener object, then call .plants on that object to get all plants associated with that Gardener. Testing this theory out in pry:

![pry1](https://drive.google.com/file/d/1AxU-wxbfDeMxXG9CvAQVBuJBvHGv6y8Z/view?usp=sharing)
![pry2](https://drive.google.com/file/d/10ubZlJN91NRTkYUV8jzJGhDFlsm-MfWK/view?usp=sharing)

I implemented another helper method and used it in my get /my_plants route:

*Helper*
```
  #grab all plants where the session[:gardener_id] == Plant.gardener_id
  def self.my_plants(session)
    @gardener = Gardener.find(session[:gardener_id])
    @my_plants = @gardener.plants
  end
```

*Route*

```get '/my_plants' do
if !Helpers.is_logged_in?(session)
redirect '/login'
end
@plants = Helpers.my_plants(session)
@gardener = Helpers.current_user(session)
erb :'/gardeners/my_plants'
end
```

This allowed me to grab just the plants that were associated with that logged in Gardener. 

This last hurdle was tricky but it helped to write everything down and execute my troubleshooting one small step at a time. Each new error revealed a bit of information I didn’t have before - this was something I could build on.

### Conclusion
Working on this project was a good reminder of how building an app like this needs careful planning and execution that is done one step at a time. I kept a doc with my user stories and tasks that needed to be completed. When I got distracted or overwhelmed, I’d come back to the doc and pick one thing to focus on. For me, projects like these never feel complete. I’m sure I’ll continue to build, refactor and improve, which is part of the fun of coding!



