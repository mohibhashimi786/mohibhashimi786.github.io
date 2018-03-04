---
layout: post
title:      "Training Wheels are OFF!"
date:       2018-03-04 06:04:47 +0000
permalink:  training_wheels_are_off
---


#"Training Wheels are OFF!"
The first project for all students in the Web Development course.  Observing fellow students who are also just about to commence their initial project in the curriculum, I see it is both a time of nervousness and excitement.  I feel both as well.  

I have never done this.  Instructions list so many requirements.  Where do I begin, what do I do?  What website do I chose to scrape?  What if I cannot scrape properly?  I have to build out the CLI, the classes, the objects, the scraper, everything….by myself.  There are no tests, how will I know if something is wrong?  I cannot ask for help on ‘ask a question’.   I understand why everyone at this point in the program is so nervous.  Now I am truly worried.

Contemplation time!  Wait, I should not have any angst.  I have been taught by the BEST PEOPLE IN THE BEST CODING SCHOOL IN THE WORLD, FLATIRON!  I have learned through all the detailed lessons, viewed live lectures, completed multiple code alongs, passed countless tests through all the labs… all the while having a community of teachers, faculty, staff and fellow students who are second to none.  They have all been here guiding, empowering, supporting and encouraging me to succeed.

I am NOT worried; I am EXCITED to see what I will come up with.  All the hard work I have put in, I know…..  I GOT THIS!

Ok, so let go through and view the ‘Daily-Deal’ example by Avi as well as the two projects done by fellow students posted on the project page.  

After a bit of pondering, I chose a video game website to scrape… I will scrape the top 100 games and then drill down into each game’s individual webpage to further scrape attributes about the respective game.  I will call it Top100games-cli.

So let us jump forward a bit (why?....it will shortly be clear)  I am about 90% done and I am testing my CLI to ensure objects and attributes are being correctly returned….when DISASTER strikes…

The website I was scraping, BLOCKED me!  




OH NO! All my hard work.  I proceed to make an appointment with the section lead, but the appointment is not for another week.  What do I do???

The section lead (like everyone else at Flatiron) was extremely accommodating and offered me an earlier appointment.  However, it still a few days away.  Despite the Disaster, I choose to make this a learning opportunity.   I decide to choose another website, “Trip Advisor” and scrape the Top 25 Beaches in the world.  

This website was a bit tricky to scrape as it was heavy with JavaScript.  The main page encompasses the top beach in the world and then contains arrows on either side to click on which in turn will then move to the next beach on the list.  Each beach’s name and image can also be clicked which then opens the individual beach’s page that lists individualized information  (That will become the attributes of my beach class and will fulfill the ‘drilling one level deep’ requirement of the project).

So here is the issue in scraping a list of the top 25, the xpath and css selector for all 25 beaches are encased in just one line.   The red number  “1” is for beach 1, and then presumably for beach 2 it would be “2” in its place and so on and so on.  

xpath:  //*[@id="TC_INNER1"]/div[1]/div[2]/div[1]/a
css selector :  #TC_INNER 1

How can I scrape all 25 without using 25 lines of code?  String Interpolation to the rescue!  I set up a BEACH constant set to an empty array, a counter and a loop to grab all 25 beaches.

def self.index_of_beaches  
    counter = 1
    while counter <= 25
    BEACHES <<  get_page.xpath("//*[@id='TC_INNER#{counter}']/div[1]/div[2]/div[1]/a")
    counter+=1
  end
  BEACHES
end

I then use the following method to scrape the ‘required’ attributes for each beach object:

def self.create_beach
  index_of_beaches.flatten.each.with_index(1) do |beach_scrape, index| 
  name = beach_scrape.text
  location = beach_scrape.xpath("//*[@id='TC_INNER#{index}']/div[1]/div[2]/div[2]/a").text
  description = beach_scrape.xpath("//*[@id='TC_INNER#{index}']/div[2]/div[1]/ul/li/div/text()").text
  url = "https://www.tripadvisor.com" +    
  beach_scrape.xpath("//*[@id='TC_INNER#{index}']/div[1]/div[2]/div[1]/a").attribute('href').value   
  Best25Beaches::Beach.new(name, location, description, url)
  end
end


Once this is done, the url attribute for each beach object then allows me to scrape individual attributes of each beach as follow:

def self.webpage_scrape
  Best25Beaches::Beach.all.each do |beach|
  webpage = Nokogiri::HTML(open(beach.url))
  beach.excellent_review_percentage ||= webpage.css("span.row_count.row_cell").text[0...3]
  beach.rating ||= webpage.css("#taplc_location_detail_overview_attraction_0 > div.block_wrap.easyClear >        
  div.overviewContent > div.ui_columns.is-multiline.is-mobile.reviewsAndDetails > div.ui_column.is-6.reviews > div.rating > span").text
  beach.activities ||= webpage.css("#taplc_location_detail_header_attractions_0 > div.rating_and_popularity >  
  span.header_detail.attraction_details > div").text
  end										
end

There are three more attributes which I would like to scrape.  When one is staying at a beach, it would be nice to know what the nearby hotels, restaurants and attractions are.  Thus, I use another method to acquire those:

def self.nearby
Best25Beaches::Beach.all.each do |beach|
webpage = Nokogiri::HTML(open(beach.url))
beach.hotels = []
beach.restaurants = []
beach.nearby_attractions = []
counter = 1
while counter <= 3
beach.hotels << webpage.xpath("//*[@id='LOCATION_TAB']/div[3]/div/div[#{counter}]/div/div[2]/div[1]").text
beach.restaurants << webpage.xpath("//*[@id='LOCATION_TAB']/div[4]/div/div[#{counter}]/div/div[2]/div[1]").text
beach.nearby_attractions << webpage.xpath("//*[@id='LOCATION_TAB']/div[5]/div/div[#{counter}]/div/div[2]/div[1]").text
counter += 1
end
end
end

Why use another method to scrape these additional attributes.  Well, as you can see I had to setup a counter, empty arrays, loops and interpolation due to the way the xpath is.  Additionally, should I choose, I could expand this project into going another level deeper into the individual hotels, restaurants and attractions’ pages to display attributes about them.  It would be scraping attributes about attributes.

At this point, I created the Beach class which would create each ‘Beach’ for me.  A CLI followed which printed the list for the user.   The user is able to choose a particular beach from the top 25 and be given individualized information about that beach as well as listing the nearby hotels, restaurants, and nearby attractions.  

I setup a loop so that the user can return to the initial menu at any point including choosing another beach and information about it.

All and all, as I said… the training wheels were off for this project.  However, I was able to learn a tremendous amount.  At times when I was stuck, I was happily surprised with how much all the work until now had paid off.  I was able to use many tools attained during the course.  

My advice to anyone who is about to begin their first project is to understand that while it is a lot of work, difficult and frustrating at times… be excited about the challenge.  Realize this is a milestone.  And if you do need help, you can make an appointment for a 30 minute session with a coach or a section lead.  There are also study groups specifically about this project, one of which I attended.  It was very helpful.

1:1 Support w/ a Support Coach (Projects only) - https://theflatironschool.typeform.com/to/UUhrc7 1:1 Support w/ a Section Lead - https://theflatironschool.typeform.com/to/Xva2Br

At the conclusion of the project you will understand that although you have a long way to go, you have also come a long way.  Conquer the moment! 



