---
layout: posts
title: Note Meetup New Haven Data Science R tutorial#2
---

## [R and R Studio Intro Workshop Series - Session #2 Data wrangling with R](https://www.meetup.com/New-Haven-Data-Science-Meetup/events/247140860/)

by IIIay Mowerman

Talked with the hoster Mike and he introduced me to others. Very welcome environment.

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

**gather**

**spread**
