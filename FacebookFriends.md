Parsing Facebook friends HTML
================

Parsing Facebook friends from HTML (not recommended)
----------------------------------------------------

Since I downloaded my data in HTML format rather than the much easier to use JSON format, I had to parse it. Here's my code for that:

``` r
here("../Documents/facebook-ameliamcnamara/friends")

facebookfriend_html <- read_html(here("../Documents/facebook-ameliamcnamara/friends", "friends.html"))

friends <- html_nodes(facebookfriend_html, "._2lel") %>%
  html_text() %>%
  as.tibble()

## write_csv(friends, "facebookfriends.csv")
```

If you are following along, you should download your data as JSON to avoid having to do this.
