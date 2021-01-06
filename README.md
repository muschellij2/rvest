
<!-- README.md is generated from README.Rmd. Please edit that file -->

# rvest <img src="man/figures/logo.png" align="right" height="139"/>

<!-- badges: start -->

[![CRAN
status](https://www.r-pkg.org/badges/version/rvest)](https://cran.r-project.org/package=rvest)
[![R-CMD-check](https://github.com/tidyverse/rvest/workflows/R-CMD-check/badge.svg)](https://github.com/tidyverse/rvest/actions)
[![Codecov test
coverage](https://codecov.io/gh/tidyverse/rvest/branch/master/graph/badge.svg)](https://codecov.io/gh/tidyverse/rvest?branch=master)

<!-- badges: end -->

rvest helps you scrape (or harvest) data from web pages. It is designed
to work with [magrittr](https://github.com/smbache/magrittr) to make it
easy to express common web scraping tasks, inspired by libraries like
[beautiful soup](https://www.crummy.com/software/BeautifulSoup/) and
[RoboBrowser](http://robobrowser.readthedocs.org/en/latest/readme.html).

If you’re scraping multiple pages, I highly recommend using rvest in
concert with [polite](https://dmi3kno.github.io/polite/). The polite
package ensures that you’re respecting the
[robots.txt](https://en.wikipedia.org/wiki/Robots_exclusion_standard)
and not hammering the site with too many requests.

## Installation

``` r
# The easiest way to get rvest is to install the whole tidyverse:
install.packages("tidyverse")

# Alternatively, install just rvest:
install.packages("dplyr")
```

## Usage

``` r
library(rvest)

# Start by reading a HTML page with read_html():
starwars <- read_html("https://rvest.tidyverse.org/articles/starwars.html")

# Then find elements that match a css selector or XPath expression
# using html_elements(). In this example, each <section> corresponds
# to a different film
films <- starwars %>% html_elements("section")
films
#> {xml_nodeset (7)}
#> [1] <section><h2 data-id="1">\nThe Phantom Menace\n</h2>\n<p>\nReleased: 1999 ...
#> [2] <section><h2 data-id="2">\nAttack of the Clones\n</h2>\n<p>\nReleased: 20 ...
#> [3] <section><h2 data-id="3">\nRevenge of the Sith\n</h2>\n<p>\nReleased: 200 ...
#> [4] <section><h2 data-id="4">\nA New Hope\n</h2>\n<p>\nReleased: 1977-05-25\n ...
#> [5] <section><h2 data-id="5">\nThe Empire Strikes Back\n</h2>\n<p>\nReleased: ...
#> [6] <section><h2 data-id="6">\nReturn of the Jedi\n</h2>\n<p>\nReleased: 1983 ...
#> [7] <section><h2 data-id="7">\nThe Force Awakens\n</h2>\n<p>\nReleased: 2015- ...

# Then use html_element() to extract one element per film. Here
# we the title is given by the text inside <h2>
title <- films %>% 
  html_element("h2") %>% 
  html_text2()
title
#> [1] "The Phantom Menace"      "Attack of the Clones"   
#> [3] "Revenge of the Sith"     "A New Hope"             
#> [5] "The Empire Strikes Back" "Return of the Jedi"     
#> [7] "The Force Awakens"

# Or use html_attr() to get data out of attributes. html_attr() always
# returns a string so we convert it to an integer using a readr function
episode <- films %>% 
  html_element("h2") %>% 
  html_attr("data-id") %>% 
  readr::parse_integer()
episode
#> [1] 1 2 3 4 5 6 7
```

If the page contains tabular data you can convert it directly to a data
frame with `html_table()`.

## Code of Conduct

Please note that the rvest project is released with a [Contributor Code
of Conduct](https://rvest.tidyverse.org/CODE_OF_CONDUCT.html). By
contributing to this project, you agree to abide by its terms.
