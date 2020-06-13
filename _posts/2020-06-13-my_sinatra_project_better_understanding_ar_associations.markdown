---
layout: post
title:      "My Sinatra Project: Better Understanding AR Associations"
date:       2020-06-13 17:12:44 -0400
permalink:  my_sinatra_project_better_understanding_ar_associations
---


In the shadow of everything that is happening in the world, my app, which tracks fast food orders, doesn't feel particularly useful.  Much more useful would have been an app that tracked racially-charged incidents of police brutality around the county and the offending officers' badge numbers for instance, but I was well underway before that struck me.  Maybe I can do that for my Rails App... 

I had a much better go of it during this second project.  I felt more comfortable with Sinatra and the underlying Ruby, even as I gained a stronger grasp on the Ruby I had learned prior.  The understanding I gained had me feeling an optimism I did not feel in the last project and the sort of optimism about understanding programming that is the driver for continuing.  But the biggest attribute as far as I could tell was the moment when the ActiveRecord associations clicked when creating this app.  I was watching a video lecture with Avi where he was discussing ActiveRecord associations between classes and he said how it can be confusing and frustrating, but as soon as it clicks you won't forget it and it will prove satisfying as well as crucial.  During this project is when I felt exactly that.  

Up until this point the examples we have used associated three classes at most.  That sort of triangular association made sense to me.  One or two belongs_to macros and a has_many or even a has_many :through macro, but I had never had to associate four or five classes.  It took a couple of crudly drawn representations, a trial with Gliffy to help illustrate and visualize those associations and more than one stereotypical head scratches.  But what was ultimately very helpful was Tux. 


<iframe width="560" height="315" src="https://www.youtube.com/embed/S2B56DhQXLw" frameborder="0"            allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

When I finally thought I had the associations figured out, the ability to play around in a console that was linked to my database was very helpful in visualizing how the associations were working and gave me that satisfying Yeatsian "[click](https://sedulia.blogs.com/sedulias_quotations/2006/01/poetry_comes_ri.html)." In the end they wound up looking like this:

```
class User < ActiveRecord::Base

    has_many :orders
    has_many :restaurants, through: :orders
    has_many :items, through: :orders
```
```
class Restaurant < ActiveRecord::Base

    has_many :orders
    has_many :users, through: :orders
    has_many :items
end
```
```
class Order < ActiveRecord::Base

    has_many :order_items
    has_many :items, through: :order_items

    belongs_to :user
    belongs_to :restaurant
end
```
```
class OrderItem < ActiveRecord::Base

    belongs_to :order
    belongs_to :item
end
```
```
class Item < ActiveRecord::Base

    has_many :order_items
    has_many :orders, through: :order_items

    belongs_to :restaurant
end
```


Looking at them now, they don't look terribly complicated anymore.  And that feels good.  After figuring it out it gave me a confidence to carry on and the ability to think more about the rest of the application and some of the more fun features to implement like validations.

My validations aren't incredibly robust, but I was able to include some within reason.  When designing my new order form, I realized that I did not want a user to be able to add duplicate items to a menu.  I considered implementing a scraper to pull official menus according to their restaurants, but it seemed to me an unnecessarily complex function for this project considering and I could always add one in the future.  Besides, menus for fast food joints across the country are not uniform.  There are special items, limited items, etcetera that I would like a user to be able to add to an order.  To prevent a menu to become sloppy with duplicate items, stylized differently e.g. "Big Mac" vs. "big mac" or "McDonalds" vs. "McDonald's" so I included simple validations to keep items and restaurants indexes clean.  There is surely more I could do, but again, for now, this is all I am really looking for.  For instance, I don't have anything preventing anyone from adding "B1g Mack" or any validation preventing the addition of a "Big Mac" to the Taco Bell menu.  So, it could still easily get sloppy very quickly.  But it was important for me to start to think about the myriad angles that have to be considered if you want to keep data and layouts clean.

Additionally, the idea of nested resources proved to be very helpful.  I maybe could have arranged my classes differently so that they weren't necessary, but I wanted a user to be able to pick a restaurant, and only then would the menu associated with the given restaurant be rendered.  I didn't know how to do that.  I solicited help during office hours and that's where the idea of nested resources was explained to me as just the tool.  A big thing that I've had to fight when learning how to program is undoing the, sort of, intuitive understanding of a website's workflow that I've learned through interacting with them as a user.  To some extenet anyway.  I imagine everyone delving into the world of programming as a longtime-listener-first-time-caller type can understand this.   Learning to use that mindset to some extent, but divorse myself from former expectations has been a valuable thing to train myself to do.  During this project I was able to balance both ways of thinking.  I am now more motivated than I have been since starting this program as I have learned to face my shortcomings, understand that I am new and learning and give up the feeling of discouragment when I am met with my limitations.  Skills will advance, acumen will accumulate and the more advanced stuff will come.

