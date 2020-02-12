---
layout: post
title:      "Creating BrewStats - my first Ruby project"
date:       2020-02-12 16:42:23 +0000
permalink:  creating_brewstats_-_my_first_ruby_project
---


#### My Approach to the project and why I built what I did

When I came to the final project and read through the requirements, it felt pretty daunting. I knew I needed to take things one step at a time and that it would be critical to outline my project before diving into coding.

I wanted to chose a topic that I'm passionate about and since I'm an avid homebrewer, I thought beer would be the perfect topic. I first looked at scraping Untapped, but chose not to use that website because of the way their data was structured. It also felt like my gem would end up being a copy of what their website is intended to do. I ended up choosing to scrape from brewersassociation.org, a resource I use frequently in my brewing hobby.

After I decided on the website I'd scrape, I started pseudocoding what I wanted my app to do. I outlined each class that I thought I'd need (turned out I added one later on) and described what each method would do. This helped me wrap my brain around each step and how I would approach writing my code.

Once I had that structure in place, I reviewed all the support material suggested by Learn - watching multiple video walk throughs, revisiting some lessons and looking at example projects. I even decided that I would start coding in my local environment versus through the IDE. I made that decision for two reasons 1) I have experience setting up local environments, coding in them and using git in my terminal and 2) I didn't want to deal with saving issues in IDE if I were to walk away from my computer for a few minutes. So before I even started my project, I went through the steps to set up my local environment. 

#### Filling in the details

With a pretty solid structure in place for what I wanted my app to do, I began filling in the details. I started with my Scraper class and used [the scraper checker tool](https://repl.it/@TheGingertonic/ScraperChecker) to work out how to scrape my website. This part definitely took some iterating but the tool was helpful since it gives you immediate feedback on what you've written. Once I had the right parent class that I'd need to iterate through, building the scraper class was pretty straightforward, especially with the help of the few walk through videos. 


#### Struggles and triumphs and discovering useful gems
One place where I ran into trouble was in copy/pasting the code from repl.it to my class and forgetting to change the notation in my each block. 

I had this in repl.it:

```
name = doc.css("#state-header").css("h1").text
```

part of my Scraper class looked like this:

```
def self.scrape_ba
    site_1 = "https://www.brewersassociation.org/statistics-and-data/state-craft-beer-stats/"
    doc = Nokogiri::XML(open(site_1))

    states = doc.css(".stat-container")

    states.each do |state|
      name = doc.css("#state-header").css("h1").text
```

Putting all of my scraped elements into the each block, I forgot to switch `doc.` with `state.` This small error led to some not helpful results and had me stuck for quite a while. I got some help during open office hours, which got me unstuck from this error and helped me more elegantly structure my scraper class.

After my Scraper class was working well, I moved on to my States class. This class is responsible for initializing my state objects, saving them and collecting them into a class variable to be used by my CLI.


I probably spent the most time building out my CLI class. Again, the video examples were really helpful in laying out some basic structure and a good approach to creating an interactive app. The methods I wanted to work with were: 
* call - which got called from my bin/brew_stats file to run the program
* get_choices - which welcomed the user and looked to my Scraper class to scrape the data and then to my States class to grab all state objects
* list_choices - iterated through the name of each state object so that I could surface those names (more on that in a bit)
* display_states - this method took in the user's selection and displayed the state information in the terminal
* next_move - allows the user to select another state (which restarts the program) or exit the program
* goodbye - called when the user wants to exit the program

##### Prompt

The challenging part about list_choices was figuring out how to elegantly show all 51 options to the user without my program listing all 51 state names in the terminal. I googled around and found a gem called [TTY::Prompt](https://github.com/piotrmurach/tty-prompt) (created by [Piotr Murach](https://github.com/piotrmurach)) which has a whole bunch of options for displaying information to a user and collecting the user's input. I read through the docs and found an option called [enum_select](https://github.com/piotrmurach/tty-prompt#264-enum_select) which allows you to provide a question (or prompt) to the user and pass in an argument of a list of choices. This gem made it possible for me to list my state options like this:

```
Select a state by it's number 
  1) Alabama
  2) Alaska
  3) Arizona
  4) Arkansas
  5) California
  6) Colorado
  Choose 1-51 [1]:
	(Press tab/right or left to reveal more choices)
```

Pressing tab or the right/left arrows would cycle through the options, keeping the output in the terminal clean and simple. One thing that took me some time to work through was figuring out how to use the selection (or output) from list_choices in my next method, display_states, which was supposed to show stats based on the user's selection. Knowing the return value of enum_select would be a string of the user's choice (ex: "Alabama"), I set my prompt.enum_select equal to an instance variable and used that instance variable in the display_states method. In this method, I use `.find` to look through my state objects and return the object whose name is == to the instance variable and then pull stats based on that.

#### Conclusion

I'm learning to code and also learning how code makes me feel - at times really clever and at other times, so confused. All said, I'm pretty happy with the way this turned out. I can imagine developing the app further, perhaps to do another scrape to get a list of all breweries for the selected state. I'll save that for another day, though.






