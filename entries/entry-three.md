# Entry Three:

In this week, I made a plan about what I was going to do with web scraping. However, I had underestimated how much total work that was, so I just finished the scraping reading instead.

## A Continuation

Picking up where I left off in the [previous entry](https://github.com/geoffreyl4412/independent-study/blob/master/entries/entry-two.md), I want to grab specific text from my website. For this example, I want to get the blue "**Sign in**" button on [Google](https://www.google.com/)!



1. First, I opened the Inspect Element tool (Command+Option+C for Mac or Control+Shift+C for Windows).
2. Next, I moved the cursor to highlight the button and clicked on it.
3. By doing so, the Elements panel highlighted the HTML for the button as shown below:



4. After locating the button's id,`#gb_70`, I then needed to call the `.css` method onto my `doc` variable. This method takes in a CSS selector and selects it, which in this case is the id.
5. If I just type `puts doc.css("#gb_70")`, this just gives the *entire element*. To get the text, I needed to add `.text` at the end of my method.

My end result is:
```ruby
require 'nokogiri'
require 'open-uri'

html = open("http://www.google.com")
doc = Nokogiri::HTML(html)
# puts doc

puts doc.css("#gb_70").text
```

This does indeed puts `Sign in` in my terminal.

Originally, I had tried to scrape the "I'm Feeling Lucky" button, but when I tried `puts doc.css("#gbqfbb").text`, *nothing* showed up in my terminal. I attempted to scrape other words too, but was unsuccessful. I'm not sure why this happened, but I moved forward anyway.

## Scraping and Iterating 

Not only can I make specific text show up, I can also *iterate* through my desired text. I believe this works because some of the HTML grabbed could have the same elements, so the result turns into an array.

This time, I decided to use the [Administration page](http://www.hstat.org/about/faculty-2/) in the High School of Telecommunication Arts and Technology (HSTAT) website.



* On line 16, `doc.css(".entry-content p")` means selecting all of the `<p>` elements within the `.entry-content` class.
* On line 20, `person.css("strong").text` selects only the text containing the `<strong>` element. 
* On the terminal, the result shown is the iteration of each administrator. The first line doesn't show a name because the first `<p>` element doesn't contain any `<strong>` elements, since it's a short paragraph describing the HSTAT administration.

[Reference to CSS Selectors](https://www.w3schools.com/cssref/css_selectors.asp)

## A Derailed Plan

When I started making my plan, I wanted to finish the scraping reading, a [scraping code along](https://github.com/learn-co-students/scraping-flatiron-code-along-001), and a [scraping kickstarter](https://github.com/learn-co-students/scraping-kickstarter-001). Unfortunately, I procrastinated again and only finished the reading this week. In the next entry, I hope to at least begin and finish the code along.

## Takeaways

1. *Tinker around and make your own unique example to fully understand what you're learning.* Instead of just reading or using the same code the reading provided, I used my own examples, like Google and the HSTAT website, to understand what specific code does.
2. *Check your code if something doesn't work.* At first, I failed to correctly iterate through my `administration` array because I forgot to use my parameter. Once I solved that problem, my code successfully worked.
3. *When planning, a **minimum viable product (MVP)** applies here.* I think I had set unrealistic goals when I planned for this week. However, with only six weeks left and using three of them on one reading, I felt the need to rush. Nonetheless, I will try to have more achievable goals in mind next time.