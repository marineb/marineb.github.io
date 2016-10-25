---
layout: post
title:  "Lookup movie showtimes near you"
date:   2016-10-25 03:32:57 +0000
---


The first assignment in the Online Full Stack program at the Flatiron School is to create a CLI ruby gem that scrapes data from a page on the internet, and do something with it. 

My first thoughts were to either create something around weather, or emojis. Since I've worked with weather data before, I wanted to do something different. And I couldn't see myself using an emoji app. 

I researched what were the [most googled keywords](http://www.siegemedia.com/seo/most-popular-keywords) in 2016. Weather, love quotes, and movies were in the first 25. I went with movies, because I could see myself using a command line (CLI) interface to lookup showtimes near me.

Googling `showtimes` used to return movie showtimes near you. But not anymore, I am not sure why. So I set to create an app that would just do that. 

The first thing I did was generating a new gem's bone structure using `bundle gem my_gem`. It automatically created the main files I was going to need. 

Then I started writing the code. As I was wrapping my head around the functionality, I kept the code within one file: `app.rb`. I first made the basics of the app work with functional programming. Once that worked, I refactored it and made it object oriented.
In addition to `app.rb`, I created:
* `scraper.rb`: scrapes the google url, and creates theater instances
* `theater.rb`: creates new theaters, with their movies / showtimes

When a new theater is added, its movies and showtimes are added too. It looks something like this:
```
theater => name,
movies => [
  {movie => name,
	  showtimes = ["2:00", "4:20", "7:00pm"]
	},
	{movie => name,
	  showtimes = ["2:00", "4:20", "7:00pm"]
	},
]
```

It makes it very easy to retrieve the data.

There are a lot of little parts of the program that I like.
This is my favorite:

```
    stars = "*" * selection.name.length
    
    puts "\n"
    puts "*************************************#{stars}"
    puts "***** Movies playing today at #{selection.name}: *****"
    puts "*************************************#{stars}"
    puts "\n"
```

It's very simple, and stupid.
It bothered me that when I was displaying the header for the list of theaters, the width of the asterisk lines wouldn't match the `Movies playing today...` line depending on what the length of the theater name was. So this is a very small addition that makes the program look much better. 

One thing to note is it took me almost as much time to program this app, as it took me to figure out how to package the gem so that people around me could download it. :)













