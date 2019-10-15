# deleting_facebook
Documentation of what I'm doing to prepare to delete my Facebook account. 

Here are the steps I've already taken:

- [Downloaded my facebook data](https://www.facebook.com/help/1701730696756992/?helpref=hc_fnav). This comes with a lot of data. Most importantly, all the photos I had uploaded to facebook over the years, organized by album, and a list of all my facebook friends. 
- [Posted on facebook](LeavingFacebook.txt) about my plan, announcing I'm starting a small (personal) newsletter, and giving a two-week countdown to get my affairs in order.
- [Parsed the HTML file of my friends list](FacebookFriends.Rmd). This HTML file is probably the least-useful way they could have provided the list of my friends, but with the help of [rvest](https://rvest.tidyverse.org/) and [SelectorGadget](https://selectorgadget.com/) I was able to find the right elements to get a data frame.
- Made a google sheet of my entire friends list, as an accessible place for the data. 
- Went through the entire list categorizing how I knew people and how much I want to stay in contact. (This took several days, as I had over 600 facebook friends!)

In progress:

- Trying to figure out how to export birthdays from facebook. It seems like you [need to use the Google API](https://github.com/mobeigi/fb2cal).

To do:

- Go through photos I'm tagged in and manually download pictures I want to keep
- Order prints of favorite photos
- Go through my google sheet again and make sure I have other modes of contact for the folks I want to stay in touch with
