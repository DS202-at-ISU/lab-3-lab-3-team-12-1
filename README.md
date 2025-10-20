
<!-- README.md is generated from README.Rmd. Please edit the README.Rmd file -->

# Lab report \#3 - instructions

Follow the instructions posted at
<https://ds202-at-isu.github.io/labs.html> for the lab assignment. The
work is meant to be finished during the lab time, but you have time
until Monday evening to polish things.

Include your answers in this document (Rmd file). Make sure that it
knits properly (into the md file). Upload both the Rmd and the md file
to your repository.

All submissions to the github repo will be automatically uploaded for
grading once the due date is passed. Submit a link to your repository on
Canvas (only one submission per team) to signal to the instructors that
you are done with your submission.

# Lab 3: Avenger’s Peril

## As a team

Extract from the data below two data sets in long form `deaths` and
`returns`

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 4.4.3

    ## Warning: package 'ggplot2' was built under R version 4.4.3

    ## Warning: package 'tidyr' was built under R version 4.4.3

    ## Warning: package 'purrr' was built under R version 4.4.3

    ## Warning: package 'dplyr' was built under R version 4.4.3

    ## Warning: package 'forcats' was built under R version 4.4.3

    ## Warning: package 'lubridate' was built under R version 4.4.3

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.1     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.2     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(readr)
library(dplyr)
```

``` r
av <- read.csv("https://raw.githubusercontent.com/fivethirtyeight/data/master/avengers/avengers.csv", stringsAsFactors = FALSE)
head(av)
```

    ##                                                       URL
    ## 1           http://marvel.wikia.com/Henry_Pym_(Earth-616)
    ## 2      http://marvel.wikia.com/Janet_van_Dyne_(Earth-616)
    ## 3       http://marvel.wikia.com/Anthony_Stark_(Earth-616)
    ## 4 http://marvel.wikia.com/Robert_Bruce_Banner_(Earth-616)
    ## 5        http://marvel.wikia.com/Thor_Odinson_(Earth-616)
    ## 6       http://marvel.wikia.com/Richard_Jones_(Earth-616)
    ##                    Name.Alias Appearances Current. Gender Probationary.Introl
    ## 1   Henry Jonathan "Hank" Pym        1269      YES   MALE                    
    ## 2              Janet van Dyne        1165      YES FEMALE                    
    ## 3 Anthony Edward "Tony" Stark        3068      YES   MALE                    
    ## 4         Robert Bruce Banner        2089      YES   MALE                    
    ## 5                Thor Odinson        2402      YES   MALE                    
    ## 6      Richard Milhouse Jones         612      YES   MALE                    
    ##   Full.Reserve.Avengers.Intro Year Years.since.joining Honorary Death1 Return1
    ## 1                      Sep-63 1963                  52     Full    YES      NO
    ## 2                      Sep-63 1963                  52     Full    YES     YES
    ## 3                      Sep-63 1963                  52     Full    YES     YES
    ## 4                      Sep-63 1963                  52     Full    YES     YES
    ## 5                      Sep-63 1963                  52     Full    YES     YES
    ## 6                      Sep-63 1963                  52 Honorary     NO        
    ##   Death2 Return2 Death3 Return3 Death4 Return4 Death5 Return5
    ## 1                                                            
    ## 2                                                            
    ## 3                                                            
    ## 4                                                            
    ## 5    YES      NO                                             
    ## 6                                                            
    ##                                                                                                                                                                              Notes
    ## 1                                                                                                                Merged with Ultron in Rage of Ultron Vol. 1. A funeral was held. 
    ## 2                                                                                                  Dies in Secret Invasion V1:I8. Actually was sent tto Microverse later recovered
    ## 3 Death: "Later while under the influence of Immortus Stark committed a number of horrible acts and was killed.'  This set up young Tony. Franklin Richards later brought him back
    ## 4                                                                               Dies in Ghosts of the Future arc. However "he had actually used a hidden Pantheon base to survive"
    ## 5                                                      Dies in Fear Itself brought back because that's kind of the whole point. Second death in Time Runs Out has not yet returned
    ## 6                                                                                                                                                                             <NA>

Get the data into a format where the five columns for Death\[1-5\] are
replaced by two columns: Time, and Death. Time should be a number
between 1 and 5 (look into the function `parse_number`); Death is a
categorical variables with values “yes”, “no” and ““. Call the resulting
data set `deaths`.

Similarly, deal with the returns of characters.

Based on these datasets calculate the average number of deaths an
Avenger suffers.

``` r
returns <- av |> 
  select(Name.Alias, starts_with("Return")) |>
  pivot_longer(
    Return1:Return5,
    names_to = "Time",
    values_to = "Returned"
  ) |>
  mutate(
    Time = parse_number(Time)
  )



head(returns)
```

    ## # A tibble: 6 × 3
    ##   Name.Alias                     Time Returned
    ##   <chr>                         <dbl> <chr>   
    ## 1 "Henry Jonathan \"Hank\" Pym"     1 "NO"    
    ## 2 "Henry Jonathan \"Hank\" Pym"     2 ""      
    ## 3 "Henry Jonathan \"Hank\" Pym"     3 ""      
    ## 4 "Henry Jonathan \"Hank\" Pym"     4 ""      
    ## 5 "Henry Jonathan \"Hank\" Pym"     5 ""      
    ## 6 "Janet van Dyne"                  1 "YES"

``` r
deaths <- av |> select(Name.Alias,
    starts_with("Death")) |>
  pivot_longer(
    Death1:Death5,
    names_to = "Time",
    values_to = "Died"
  ) |>
  mutate(
    Time = parse_number(Time)
  )

head(deaths)
```

    ## # A tibble: 6 × 3
    ##   Name.Alias                     Time Died 
    ##   <chr>                         <dbl> <chr>
    ## 1 "Henry Jonathan \"Hank\" Pym"     1 "YES"
    ## 2 "Henry Jonathan \"Hank\" Pym"     2 ""   
    ## 3 "Henry Jonathan \"Hank\" Pym"     3 ""   
    ## 4 "Henry Jonathan \"Hank\" Pym"     4 ""   
    ## 5 "Henry Jonathan \"Hank\" Pym"     5 ""   
    ## 6 "Janet van Dyne"                  1 "YES"

``` r
totaldeaths = nrow(filter(deaths, Died=='YES'))
#totaldeaths

totalavengers = length(unique(deaths$Name.Alias))
#totalavengers

avg = totaldeaths/totalavengers
avg
```

    ## [1] 0.5460123

## Individually

For each team member, copy this part of the report.

Each team member picks one of the statements in the FiveThirtyEight
[analysis](https://fivethirtyeight.com/features/avengers-death-comics-age-of-ultron/)
and fact checks it based on the data. Use dplyr functionality whenever
possible.

### FiveThirtyEight Statement

> Quote the statement you are planning to fact-check.

Allison: “I counted 89 total deaths”

Ivy: “My analysis found that 69 had died at least one time after they
joined the team.”

Norah: “on 57 occasions the individual made a comeback”

### Include the code

Make sure to include the code to derive the (numeric) fact for the
statement

Allison:

Ivy:

``` r
died_once <- deaths %>%
  mutate(died = Died == "YES") %>% 
  group_by(Name.Alias) %>%
  summarise(n_deaths = sum(died, na.rm = TRUE), .groups = "drop") %>%
  summarise(n = sum(n_deaths >= 1))



died_once
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1    64

Norah:

``` r
head(returns)
```

    ## # A tibble: 6 × 3
    ##   Name.Alias                     Time Returned
    ##   <chr>                         <dbl> <chr>   
    ## 1 "Henry Jonathan \"Hank\" Pym"     1 "NO"    
    ## 2 "Henry Jonathan \"Hank\" Pym"     2 ""      
    ## 3 "Henry Jonathan \"Hank\" Pym"     3 ""      
    ## 4 "Henry Jonathan \"Hank\" Pym"     4 ""      
    ## 5 "Henry Jonathan \"Hank\" Pym"     5 ""      
    ## 6 "Janet van Dyne"                  1 "YES"

``` r
comebacks <- count(returns, Returned=='YES')
comebacks
```

    ## # A tibble: 2 × 2
    ##   `Returned == "YES"`     n
    ##   <lgl>               <int>
    ## 1 FALSE                 808
    ## 2 TRUE                   57

``` r
newcb <- filter(comebacks, comebacks$`Returned == "YES"`==TRUE)
newcb
```

    ## # A tibble: 1 × 2
    ##   `Returned == "YES"`     n
    ##   <lgl>               <int>
    ## 1 TRUE                   57

### Include your answer

Include at least one sentence discussing the result of your
fact-checking endeavor.

Upload your changes to the repository. Discuss and refine answers as a
team.

Allison:

Ivy: Based on my analysis of the dataset, **64 Avengers** have died at
least one time after joining the team. This number is slightly lower
than the FiveThirtyEight claim of 69 deaths. The difference may be due
to dataset updates or variations in how deaths were counted.

Norah: Based on the code that I used to analyze the returns dataset,
there were in fact 57 times that an individual died and returned. This
exactly matches the number of times that the article claims people
returned from the dead.
