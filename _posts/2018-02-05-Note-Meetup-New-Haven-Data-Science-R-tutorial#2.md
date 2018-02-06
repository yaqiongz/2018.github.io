---
layout: post
title: Note Meetup New Haven Data Science R tutorial#2
---

[meetup: New Haven Data Science](https://www.meetup.com/New-Haven-Data-Science-Meetup/events/247140860/)
### R and R Studio Intro Workshop Series - Session #2
### Data wrangling with R
by IIIay Mowerman

This is the first meetup I've ever attend. I talked with the hoster Mike and he introduced me to others. And also meet another person who is in a data science bootcamp. This is very welcome environment. 

The tutorial focused on R and data wrangling. Those functions are talked and followed by several excercises.

### Dataset: Affair form AER.

load the data set:

data("Affair")

affairs <- Affairs

### librarys used in the tutorial:

[AER](https://crantastic.org/packages/AER)

[tidyverse](https://www.tidyverse.org/)

### Functions we used:

**arrange**

n1 <- affairs %>% arrange(desc(affairs),religiousness)

same with:

n1 <- arrange(affairs, desc(affairs))

**filter**

n2 <- affairs %>% filter(rating >2, rating<4)

**mutate**

*this function can add columns(variable) based on the exist colunms(variable)*

n3 <- cheater %>% mutate(cheater = ifelse(affairs>=6,1,0),religiouse_rating = religiousness/rating )

**mean**

mean_cheater <- mean(affair$cheater)

**str**

structure

**summarise**

n4 <- cheater %>% summarise(sum_cheater=sum(cheater),mean_rating=(rating))

**group_by**

n5 <- cheater

**ungroup**

**select**

cheater %>% select(start_with('r'))

**rename**

**inner_join**

**left_join**

left <- left_join(A,B)

all the member from A

**semi_join**

only use A for inner join

**anti_join**

The item in A not in B

**gather**, **spread**
