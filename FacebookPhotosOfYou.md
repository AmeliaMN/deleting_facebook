Scraping tagged photos
================

## Tagged photos of me

Another goal was to get tagged “photos of you” pictures backed up.
Facebook really doesn’t want you to do this. I tried several approaches.
(Some that failed are detailed in UselessScraps.Rmd)

### RSelenium

Selenium is good because it drives a real browser. This is the approach
the Python scripts for scraping tagged photos take. Those scripts don’t
work because Facebook has changed the HTML, but I figured I could debug
the code better in R.

Since I don’t know about Docker, I just used the `rsDriver` function to
navigate.

``` r
library(RSelenium)
rD <- rsDriver(port = 4445L, browser = "firefox", version = "3.141.59")
remDr <- rD$client
```

I was able to get logged in pretty easily.

``` r
remDr$navigate("https://www.facebook.com/")

YourLogIN <- ""
YourPassword <- "" # this is where your password goes

remDr$findElement("id", "email")$sendKeysToElement(list(YourLogIN))
remDr$findElement("id", "pass")$sendKeysToElement(list(YourPassword))
remDr$findElement("id", "loginbutton")$clickElement()
```

The first step is to navigate to photos of me. I am lazy, so I just
hardcoded this to my facebook URL. There is probably a way to grab this
automatically, but we’re aiming low here.

``` r
remDr$navigate("https://www.facebook.com/amelia.mcnamara/photos_of")
```

Then, I click on the first tagged photo of me to get the slideshow to
start.

``` r
webElem <- remDr$findElement(using = "class", "uiMediaThumb")
webElem$clickElement()
```

It would be handy if we could right-click with RSelenium to download the
files. But, it doesn’t work. So instead we’re just going to grab the src
links and do the downloading separately.

I tried a couple approaches with while statements that did not work. So,
here is another hacky solution:

  - make a character vector of length 5000 (seemed likely to be a much
    greater number than the number of tagged photos of me)
  - run a for loop to iteratively grab the src and click on the next
    button
  - checked to see if the current src was the same as the first (photos
    eventually loop back around) and if so, stop

<!-- end list -->

``` r
srcs <- character(5000)

## first two
for (i in 1:2) {
  img1 <- remDr$findElement(using = "class", "spotlight")
  srcs[i] <- img1$getElementAttribute("src")[[1]]
  nextButton <- remDr$findElement(using = "class", "next")
  nextButton$clickElement()
}

for (i in 3:5000) {
  img1 <- remDr$findElement(using = "class", "spotlight")
  srcs[i] <- img1$getElementAttribute("src")[[1]]
  if (srcs[i] == srcs[1]) {
    stop(paste("done, with", i-1, "photos"))
  }
  nextButton <- remDr$findElement(using = "class", "next")
  nextButton$clickElement()
}
```

Then, I made a tibble with those urls, and made sure to just grab the
distinct ones. Then I made up some names for the files I was about to
download. I saved this dataframe to an external file so I wouldn’t lose
those valuable URLs\!

``` r
srcs_df <- tibble(url = srcs)
srcs_df <- srcs_df %>%
  distinct(url)
srcs_df <- srcs_df %>%
  mutate(filename = paste0("img/FB_download", row_number(), ".jpg"))

write_csv(srcs_df, "fb_images.csv")
```

To finish up with RSelenium, you need to close the session,

``` r
remDr$close()
```

Somehow this never fully works, so I end up having to go into Activity
Monitor and kill the Java process.

## Downloading images

The final step is to download the images. I ended up with 878 tagged
photos on facebook, so again I did a for loop (perhaps this could have
been vectorized?).

``` r
library(curl)
srcs_df <- read_csv("fb_images.csv")
for (i in 1:878) {
  curl_download(srcs_df$url[i], srcs_df$filename[i])
}
```

Success\! I now have all the files on my local computer. And, if I want
to grab them on a different computer I have the csv of URLs (which don’t
seem to have time-expirations in them, as some facebook URLs do) to run
the `curl` stuff again.
