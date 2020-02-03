# Documentation of what I did to prepare to delete my Facebook account. 

If you want a more narrative view of this whole thing, check out [the blogpost I wrote about the whole process](https://www.amelia.mn/blog/misc/2019/12/29/Deleting-Facebook.html). Otherwise, see the bullets below. 

Here are the steps I took:

- [Downloaded my facebook data](https://www.facebook.com/help/1701730696756992/?helpref=hc_fnav). This comes with a lot of data. Most importantly, all the photos I had uploaded to facebook over the years, organized by album, and a list of all my facebook friends. Initially, I unchecked some boxes in the facebook data, so I hadn't downloaded the list of Apps and Websites I used facebook to log in with. I eventually went back and grabbed those. 
- [Posted on facebook](LeavingFacebook.txt) about my plan, announcing I'm starting a small (personal) newsletter, and giving a two-week countdown to get my affairs in order.
- [Parsed the HTML file of my friends list](FacebookFriends.md). This HTML file is probably the least-useful way they could have provided the list of my friends, but with the help of [rvest](https://rvest.tidyverse.org/) and [SelectorGadget](https://selectorgadget.com/) I was able to find the right elements to get a data frame. [Edit! I missed the option to dowload your data in JSON format, which is much easier to parse than the HTML. This would be my recommended method.]
- Made a google sheet of my entire friends list, as an accessible place for the data. 
- Went through the entire list categorizing how I knew people and how much I want to stay in contact. (This took several days, as I had over 600 facebook friends!)
- Exported my friends' birthdays from facebook. Facebook has changed their settings, so it's no longer easy to do this. But, [mobeigi has a python script](https://github.com/mobeigi/fb2cal) that will do it for you. As he warns, using this script made facebook temporarily suspend my account because of suspected security issues. I was able to get back in.
- [Downloaded all the photos I am tagged in](FacebookPhotosOfYou.md).  There were methods available on the internet using Python (see, [gnmerritt](https://gnmerritt.net/deletefacebook/2018/04/03/fb-photos-of-me/), [jcontini](https://github.com/jcontini/fb-photo-downloader), and [KatyaRuth](https://github.com/KatyaRuth/photos-of-you)), but none of them worked with the current version of facebook (I think Facebook likely changed their API to make it more difficult to scrape this stuff). I'm not a Python person, so it was hard for me to debug what was not working about them. I wrote [my own code using RSelenium](FacebookPhotosOfYou.md) to do the browser stuff. It is highly inefficient, but it works!  
- Made sure the Apps and Websites I used facebook to log in to have alternate modes of access. 
- Go through my google sheet again and make sure I have other modes of contact for the folks I want to stay in touch with. [Didn't do a great job of this, but it's time to let go!]
- **Actually deleted my account!!**
- [Parsed the birthday .ics file](parsing_birthdays.md) to save just the birthdays of people I want to stay in contact with. (I don't need 600+ birthday reminders on my calendar!)
- Ordered prints of favorite photos from Spoonflower, organized them into a photo album.
- Cleared cookies from my work computer, home computer, and phone
- Wrote [a blog post](https://www.amelia.mn/blog/misc/2019/12/29/Deleting-Facebook.html) about my entire process


## What am I going to use instead? 

- I'm still going to be active on Twitter. I have a LinkedIn account. And, for right now I'm keeping Instagram. I know Instagram is owned by facebook, but it just feels like too much to delete IG right now. Baby steps!
- I'm starting a personal newsletter to send updates to my friends and family. I'm using [TinyLetter](https://tinyletter.com/) for this.
- To keep up on local events, I'm planning to pay closer attention to local arts publications, like the [City Pages](http://www.citypages.com/) in the Twin Cities. 
- For events I'm hosting, I'll continue to use [Paperless Post](https://www.paperlesspost.com/) to invite folks (I haven't used FB for event invites in a couple of years).
- Mostly, I'm going to be emailing, texting, calling, and talking face-to-face with my social network! 

