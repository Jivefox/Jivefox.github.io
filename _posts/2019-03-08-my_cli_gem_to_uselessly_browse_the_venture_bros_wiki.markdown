---
layout: post
title:      "My CLI Gem to Uselessly Browse The Venture Bros. Wiki!"
date:       2019-03-08 20:20:36 +0000
permalink:  my_cli_gem_to_uselessly_browse_the_venture_bros_wiki
---


It was a rough go of it.  My experience coding my CLI gem project has been all of the anguish and little to none of the relief expressed by my fellow cohort members.  My familiarity with failure had been one of controlled self-protection before this project.  I had rigged it so that I might pass off any failure as a willing forfeiture of my time and effort and that if I had only carved out a little time, made a tiny push to succeed, I would have.  And I have found solace in the success I have had without devoting much effort as evidence that I would flourish were I to ever give a shit.  For the first time, I was actually a responsible student and used my waking moments before and after work, with the assistance of variable mental accelerants, to work on my code.  And it is sort of a mess and, sort of, a broken failure, but I did it.  And am willing to move on and try the next thing, so I have accepted a perverse sense of pride over my resolve to keep going and keep failing.  Okay, that little disclaimer was mostly for me as ventilation, but could also, hopefully, serve as some little bit of motivation to those who may also feel the same sting of disheartenment.  Here's a little something about my Gem:

Not unique to my project, I ran into many hiccups in my search for a good candidate site to scrape.  Many sites that seemed to have a layout that would translate well into Ruby classes and easily relatable objects wound up lousy with Javascript.  And honestly I had scant idea of what would be useful information to scrape.  I settled on data that would be useful to exactly no one, but that I might be able to tolerate looking at since for the lengthy amount of time I was sure to be spending with the material.  Go ahead, give it a shot, this is the way to fatigue yourself of any interest that you may worry is becoming a problematic obsession.  Maybe designing a CLI gem to scrape data from Netflix would cure many Americans from their television binges!  

The Venture Bros.  wiki wound up being sort of a nuisance to scrape in its own right.  The data I was interested in scraping was stored on each page in classless tables (I swear I remember seeing a recorded video lesson with Avi that covered scraping data from tables, but for the life of me I could not relocate that video.  And now that I have fallen behind and am familiarizing myself with SQL and tables a little better, I may have a better shot at it when I return to clean it up a bit).

![](http://gdurl.com/4g1r)

Initially, I thought I'd solve that issue by directing Nokogiri to a specific index after iterating over a given nodeset and collecting that data in an array.  The data I wanted seemed to be in uniform structure at first glance.  That turned out not to be the case.  Nearly every page did, in fact, hold the same indexicality for the data I was after, but the few that deviated from that form caused my program to break.  So, I had to point Nokogiri to the children of a given table and then use an index as long as the text within the given node matched a "Voiced By" string.  This required way too much time and the parsing of too many snarky answers on StackOverflow, but eventually Z stepped in and saved me from self-harm.  My code to scrape the voice_actor—which was a particularly pesky little bit for me—wound up looking like this:

```
 def self.scrape_character_page(character)
    html = open(VENTURE_URL + character.name.gsub(" ","_"))
    page = Nokogiri::HTML(html)
    character_details = {}
    page.css('table.infobox tr')[4..-1].detect do |nodeset|
      nodeset.children.find do |node|
        character_details[:voice_actor] = nodeset.children.last.text.strip if node.text.include?("Voiced by")
      end
    end
    character_details[:first_appearance] =  page.css('tr td a').map {|object| object.attr('title')}[2]
    character_details[:episodes] = page.css('ul li i').map {|object| object.text}
    character_details
  end
```

I was able to scrape information from the wiki to create character instances with a record of the episode in which they first appeared, who voiced that character and other episodes in which they also appeared.  I was then able to create instances of an Episode class and with an associated air-date, season and episode index populated by information scraped from the episode pages of the wiki.  Why is this useful?  It isn't!  But, if you want to bone up on Venture Bros. trivia, it is just the gem you're looking for!  And in all sincerity, I feel perversely encouraged by my close brush with despondency when coding this mess of a gem.  Every project can't be this destructive to the ol' ego, can it?

### Future Ambition for this Gem

I can't say I have much in mind for the progression of this project for any real-life application, but I would like to revisit it—likely recode entirely and overhaul it—so that I can get it working without the same wrinkles simply for my own satisfaction.  I would like to give the class relationships slightly more functionality so that their data is less discrete and more dynamic.  Mapping the Ruby classes to SQL databases would probably make some amount of sense since the data is laid out that way already.  The ability to retrieve statistics based on the object relationships would provide a slightly more useful gem in the odd chance that a user's local pub were to host a Venture Bros. Geeks Who Drink edition.  Some information that would be nice to have:

* Characters who appear in the most episodes
* Episodes in which the most characters are intoduced
* Episodes that host the largest voice-acting cast
* Organizing characters by gender
* Which episodes pass the Bechdel Test

But for now, I'm moving onward and upward or lateralward so enjoy the basic functionality!


