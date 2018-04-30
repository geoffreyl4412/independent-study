# Entry Five:

In this week, I continued onward to learning how to scrape and store the data into a hash. Then, I made another example of what I learned, using a different webpage.

## My Example Again

Like the previous entry, after I finished doing the [scraping kickstarter](https://github.com/geoffreyl4412/scraping-kickstarter-001), I chose to make my own example to show off what I have learned. **However**, unlike the last entry's example, the kickstarter scraped from an HTML document, not a website. I decided that my example should use a website since it is much more convenient for me. 

This time, I scraped my data from [Google's offices in North America](https://www.google.com/about/locations/?region=north-america).



## Starting and Using `binding.pry`

```ruby
require 'nokogiri'
require 'open-uri'
require 'pry'

def create_locations_hash
  html = open("https://www.google.com/about/locations/?region=north-america")
  google = Nokogiri::HTML(html)
  
  binding.pry
end

create_locations_hash
```

As usual, I started off with these lines of code to scrape. The `binding.pry` is, once again, being used to find the desired CSS selectors to scrape data from. The last line, `create_locations_hash` is needed in order to run Pry. 

After running the file in my terminal, I used my inspect element tool on the website. *There, I saw each location had an* `offices-list` *class in an* `li`.



To check if I was correct, I typed in my terminal `google.css("li.offices-list").first` to only get the first location. This does indeed give me the first location.

**A cool thing that I learned from the kickstarter was that in Pry, you can assign a variable name to the latest return value by typing** `your_variable_name = _`. In this scenario, this is useful for saving time, instead of typing `google.css("li.offices-list").first` over and over again. I named my variable `office`, since the line of code returns the first office.

## Finding the Rest of the Information

The first thing I want to find is the city of each location. I used the inspect element tool again and found that they lived in an `h2` with an `office-name` class (no pun intended).

Checking again by typing `office.css("h2.office-name").text` in Pry gives me the city of the first office. I also tried typing `google.css("li.offices-list").first.css("h2.office-name").text` (without using my `office` variable) and **it still gave me the same result**, which makes sense. 



Moving forward, I want to scrape each image link, address, and phone number. To reiterate, I scrape them by following these same two steps:

1. Use the inspect element tool on the web page to find the CSS selectors.
2. Once I find it, go to Pry in the terminal and check if they scrape the correct information.

My end result for all of the desired texts are as follows: 

```ruby
offices: google.css("li.offices-list")
city: office.css("h2.office-name").text
image link: office.css("div.office-photo img").attribute("src").value
address: office.css("div.office-address").text
phone number: office.css("span.phone-number").text
```

The `.attribute` method for the image link is getting the value of `"src"`.

## The Home Stretch

Now that I have the CSS selectors of the information that I want, I need to iterate through each location. I also need to create a hash to store my result.

```ruby
def create_locations_hash
  html = open("https://www.google.com/about/locations/?region=north-america")
  google = Nokogiri::HTML(html)
  
  locations = {}
  
  google.css("li.offices-list").each do |location|
    locations[location] = {}
  end
  
  locations
end
```

After that, I need to make each city equal to a key and turn them into symbols. I also need to make the value be a hash of the information for that city. The code above wouldn't work because the key would be a Nokogiri object.

```ruby
google.css("li.offices-list").each do |location|
    city = location.css("h2.office-name").text
    locations[city.to_sym] = {}
end
```

After tying up some loose ends, my final result is shown below.

```ruby
require 'nokogiri'
require 'open-uri'
require 'pry'

def create_locations_hash
  html = open("https://www.google.com/about/locations/?region=north-america")
  google = Nokogiri::HTML(html)

  locations = {}
  
  # offices: google.css("li.offices-list")
  # city: office.css("h2.office-name").text
  # image link: office.css("div.office-photo img").attribute("src").value
  # address: office.css("div.office-address").text
  # phone number: office.css("span.phone-number").text
  
  google.css("li.offices-list").each do |office|
    city = office.css("h2.office-name").text
    locations[city.to_sym] = {
      :image_link => office.css("div.office-photo img").attribute("src").value,
      :address => office.css("div.office-address").text,
      :phone_number => office.css("span.phone-number").text
    }
  end
  
  locations
end

puts create_locations_hash
```

## Takeaways

1. *Test out lines of code and see what result is.* I didn't know if typing `google.css("li.offices-list").first.css("h2.office-name").text` would be any different than typing `office.css("h2.office-name").text` until I tried them in the terminal. I then learned that they both return the same thing. 
2. *Remember the "syntax/process" of what you are making so that you know how to make something with it next time.* In the last two entries, I've pretty much done the same process of how to find my CSS selectors to scrape. I now remember that process so that I don't have to refer back to other entries on how to do this.