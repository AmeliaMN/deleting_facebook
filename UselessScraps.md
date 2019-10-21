Facebook data scraping scraps (non-useful)
================

This is a place to dump stuff that doesn't work, but I don't want to fully lose in case I needed to come back to it.

### rvest

Initially, I just inspected a downloaded HTML source file

``` r
photos_of_me <- read_html(here("../Downloads", "view-source_https___www.facebook.com_amelia.mcnamara_photos_of.htm"))
```

Using `rvest`, I was able to get a little information out of the HTML.

``` r
photo_urls <- photos_of_me %>%
  html_nodes(xpath ='//comment()') %>%
  html_text() %>%
  paste(collapse = '') %>%
  read_html() %>%
  html_nodes(".uiMediaThumb") %>%
  html_attrs() %>%
  map_dfr(as.list)
```

But, doing any sort of live login for facebook through `rvest` failed, because Facebook doesn't recognize the browser.

RSelenium, not useful
---------------------

Tried just scrolling to the end of the page to grab the HTML

``` r
webElem$sendKeysToElement(list(key = "end"))
# page1 <- read_html(remDr$getPageSource()[[1]]) 
page1 <- read_html(remDr$getPageSource()[[1]]) %>%
  html_nodes(xpath ='//comment()')
XML::getNodeSet(page1, path = '//comment()')
remDr$findElement(using="class", "uiMediaThumb")
```

lots of attempts to right-click, all of them unsuccessful

``` r
remDr$click(2)
remDr$sendKeysToActiveElement(
list(key = 'down_arrow', key = 'down_arrow', key = 'enter')
)
img1$highlightElement()
img1$getElementAttribute('value')
img1$getElementAttribute('innerHTML')
remDr$mouseMoveToLocation(x = 500, y = 500)
remDr$click(2)
remDr$sendKeysToActiveElement(
list(key = 'down_arrow', key = 'down_arrow', key = 'enter')
)
remDr$buttondown(buttonId = 2)
remDr$click(buttonId = 2)
```

``` r
img1$sendKeysToElement(list(key = 'right_arrow', key = 'down_arrow', key = 'down_arrow',  key = 'down_arrow', key = 'enter'))
```

``` r
img1 <- webElem$findElement(using = "class", "spotlight")
img1$getElementAttribute("src")
```

``` r
myFun_2 <- function(inlist) {
  x <- as.data.frame(do.call(rbind, inlist))
  x[] <- lapply(x, unlist)
  x
}

src_df2 <- myFun_2(srcs)
src_df2 <- src_df2 %>%
  mutate(url = as.character(V1))
```

splashr
-------

Since I can't figure out how to right-click in RSelenium, and I haven't been able to successfully download the HTML source to parse it differently, I decided it was probably time to try Docker. This will allow me to use splashr, which is perhaps a more appropriate package.

``` r
library(splashr)
lua_ex <- '
function main(splash)
  splash:go("http://rud.is/b")
  splash:wait(0.5)
  local title = splash:evaljs("document.title")
  return {title=title}
end
'

splash_local %>% execute_lua(lua_ex) -> res
rawToChar(res) %>% 
  jsonlite::fromJSON()
```

``` r
lua_ex <- '
function main(splash)
  assert(splash:go("https:://facebook.com/"))
  assert(splash:wait(0.5))
  splash:send_text("amelia.a.mcnamara@gmail.com")
  splash:send_keys("<Tab>")
  splash:send_text("")
  splash:send_keys("<Tab>")
  splash:send_keys("<Return>")
  splash:wait(0.5)
  assert(splash:go("https://www.facebook.com/amelia.mcnamara/photos_of"))
  splash:mouse_press(150, 500)
  return {har=splash:har()}
end
'

splash_local %>% execute_lua(lua_ex) -> res

blergh <- rawToChar(res) %>% 
  jsonlite::fromJSON()

print(blergh)
```

This suggests that I'm using an unsupported browser once again. Maybe it's time to go back to RSelenium but use Docker?
