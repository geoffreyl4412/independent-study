# Entry Seven:

In this week, I started to build my MVP, but I ran into a wall. I came up with a new idea since the previous didn't work.

## First Things First

In order to actually build my MVP, I first have to clone my app-template from Sinatra. I then renamed the folder `scraping-project`. Next, I went to my `model.rb` file and typed the following lines of code: 

```ruby
require 'nokogiri'
require 'open-uri'
require 'pry'

def scrape_yt_results(info_type)
    html = open(yt_search)
    yt_results = Nokogiri::HTML(html)
end
```

Using the inspect element tool in a random search on YouTube, I was able to find the locations of these attributes:

```ruby
#each result is in class="style-scope ytd-item-section-renderer"
#each title is in id="video-title" and class="yt-simple-endpoint style-scope ytd-video-renderer"
#each channel name is in class="yt-simple-endpoint style-scope yt-formatted-string"
#each view count is in class="style-scope ytd-video-meta-block"
#each upload date is in class="style-scope ytd-video-meta-block"
#each description is in id="description-text" and class="style-scope ytd-video-renderer"
#each thumbnail is in id="img" and class="style-scope yt-img-shadow"
```

## A Roadblock

In my scraping sandbox, I tried to scrape the information above, but nothing came up. I used the same `binding.pry` strategy that I used in previous entries, but when I typed `yt_results.css(".style-scope.ytd-item-section-renderer")` for example, only an empty array was returned. I tried the other classes, and even went to try my other ideas for scraping YouTube. However, I kept on getting the same result: an empty array. Just typing `yt_results` gave me the whole HTML of the page, so I knew that Nokogiri and Open-Uri wasn't the issue.

I think this happened because in the website, there were multiple attributes that had the same or similar classes, so I guess the program got confused over which class to choose from.

## Another Idea

While I was thinking about what to do for my MVP, I remembered Mr. Mueller told me about how last year a student made a program to ask trivia questions and scraped from Wikipedia. My project could be to scrape information on Wikipedia based on what the user searches for. I used a random Wikipedia page and used the inspect element tool and `binding.pry`. Finally, lo and behold, I successfully grabbed the HTML from a website. Just like YouTube's url, Wikipedia's url is just what the user searches for.

My next steps are to have a MVP done by the next entry.

## Takeaways

1. *If you want inspiration, look back at previous projects.* If I didn't know about the trivia program, I would've still been stuck on what to build.
2. *Test out your code to see if it will work.* I should've done that before I decided what to make, and now I still don't have an MVP.