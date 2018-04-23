# Entry Four:

In this week, I completed the code along and made two similar classes from that to display what I got from the code along. I also discover how helpful Pry is in scraping websites.

## My example

After I completed the code along, I decided to make my own example to show off what I have learned. Instead of scraping from the [Flatiron School's Courses website](http://learn-co-curriculum.github.io/site-for-scraping/courses), I scraped from a different website. However, I browsed through many websites like [Amazon](https://www.amazon.com/) and [Apple](https://www.apple.com/), but unfortunately did not find one that was similar to the Flatiron School's. After some more searching, I finally came across [Netflix's website](https://www.netflix.com/) and decided to use that one. 

In the code along, I had to scrape the course's `title`, `schedule`, and `description`. In my example, I chose to scrape from the "**Watch anywhere**" tab shown below:



I named the texts `device` and `category`. After that, I made my classes.

## The `Netflix` class

I first made the `Netflix` class, which will create the instances. I gave `device` and `category` both getters and setters. Then, I made a class variable, `@@all` and set it equal to an empty array. *Initializing `@@all` enabled me to store instances of when the class is initialized*. I also added the class method `.all` to make `@@all` visible. My end result is shown below:

```ruby
class Netflix
    attr_accessor :device, :category
    
    @@all = []

    def initialize
        @@all << self
    end
    
    def self.all
        @@all
    end
end
```

## Creating the `Scraper` class

After requiring the necessary gems and the `Netflix` class, I started building the `Scraper` class, which is responsible for scraping and instantiating. I first made the `get_page` method, which just scrapes the HTML of the website.

```ruby
def get_page
    doc = Nokogiri::HTML(open("https://www.netflix.com/"))
end
```

Next, I used the inspect element tool, and saw that each section of text was inside `.tin-small-desc`. 



## Finding with `binding.pry`

To find what CSS selector the bolded text was in each section, I placed a `binding.pry` at the end of `get_page`. I ran the ruby file, and in the terminal, I typed `doc.css(".tin-small-desc")`, returning elements of `.tin-small-desc`. Then, I only got the first element by typing `doc.css(".tin-small-desc").first`. In the terminal I could see my desired text. 



The bold text and unbolded text are in `h3` and `span` respectively. I checked if this was true by typing `doc.css(".tin-small-desc").first.css("h3")` and `doc.css(".tin-small-desc").first.css("span")`, which indeed gives the element of the selector. I made the finishing touch by adding `.text` at the end of what I typed before, giving me the desired text.

## Iterating the elements

I took a step further and the text in each variable by adding this code in `get_page`:

```ruby
doc.css(".tin-small-desc").each do |section|
    description = Netflix.new
    description.device = section.css("h3").text
    description.category = section.css("span").text
end
```

This means that in each element in `.tin-small-desc`, make a new instance of `Netflix` with `device` and `category`. I could see this in action by placing a `binding.pry` again after `end` and typing `Netflix.all` in the terminal.

## Finishing `Scraper`

After commenting out the previous lines of code, I made the `get_descriptions` method, which uses a CSS selector from the HTML grabbed from `get_page` to return the sectioned text. 

Next, I made the `make_descriptions` method, which is the previous lines of code I had about iterating. 

Finally, I defined the `print_descriptions` method, which uses `make_descriptions` to iterate the text and `puts` it. 

Removing the `doc` variable and adding `Scraper.new.print_descriptions` after the `Scraper` class, my final result is below:

```ruby
require 'nokogiri'
require 'open-uri'
require 'pry'

require_relative './watch_anywhere.rb'

class Scraper
    
    def get_page
        Nokogiri::HTML(open("https://www.netflix.com/"))
    end
    
    def get_descriptions
        self.get_page.css(".tin-small-desc")
    end
    
    def make_descriptions
        self.get_descriptions.each do |section|
          description = Netflix.new
          description.device = section.css("h3").text
          description.category = section.css("span").text
        end
    end
    
    def print_descriptions
        self.make_descriptions
        Netflix.all.each do |description|
            if description.device
                puts "Device: #{description.device}"
                puts "Description: #{description.category}"
            end
        end
    end
end

Scraper.new.print_descriptions
```
The hardest part of showing what I learned was finding an appropriate website to scrape from with a similar formatting as the code along.

## Takeaways

1. *If you can, use `binding.pry`*! I had never known Pry was useful until I did the code along. Pry is great at testing code to see what shows up in the terminal.
2. *Don't give up on what you're searching for*! In the beginning, of the entry, I mentioned how I had to scrape a website that was similar to the code along's, but didn't find one for a while. I first tried the [HSTAT's academic page](http://www.hstat.org/academics-2/), but the CSS selectors were not suitable for scraping. Nonetheless I didn't give up and managed to find Netflix as my website to scrape.