# Report

Theory: A chi-squared test with the same value for each slot will yield an infinite number, due to it being exactly the same, and you cannot prove an effect in any way.

I checked my theory, in which I thought a chi-squared test where each square has the same number in it, would yield a low value of independence. I thought this would be true, however I had forgotten that a high p value means a high independence with no correlation, and not a low value.
I ran a chi-squared test with men and women, and the number of cat/dogs each group had. I put a '5' for each value, and the results of this yielded a p value of 1. Since there is obviously no correlation between gender and choice of pet.

``` R
#Code for Table 1
library(stats)
data <- matrix(c(5, 5, 5, 5), nrow = 2, byrow = TRUE)
colnames(data) <- c("Men", "Women")
rownames(data) <- c("Cats", "Dogs")
chi_square_result <- chisq.test(data)
print(chi_square_result)

#Code for Table 2
library(stats)
data <- matrix(c(10, 0, 0, 10), nrow = 2, byrow = TRUE)
colnames(data) <- c("Men", "Women")
rownames(data) <- c("Cats", "Dogs")
chi_square_result <- chisq.test(data)
print(chi_square_result)
```
Table 1
.  |Male  |  Female
---|------|--------
Cat|  5   |    5   
Dog|  5   |    5   

> This table returns a p value of 1.
    
Table 2
.  |Male  |  Female|
---|------|--------|
Cat|  10  |    0   |
Dog|  0   |    10  |
>This table returns a p value of 5.699e-05

Tried to load data from teaching_hours.csv, but ran into issues.

Copied Richels code for loading the data from the analysis, however ran into a wall when trying to run a chi-squared test on it. I could not for the life of me think out a way to do it myself, and when trying to copy Richels test, it returned the error: Error in sum(x) : invalid 'type' (character) of argument. I also added tables to previous part of report, to clarify what I was doing.

Tried running chi-squared tests on the data from the analysis, code below. I don't know if any of these are the correct way to do it, but I'm doing something atleast.
```R
chisq.test(x = t_hours_per_type_per_gender$female, y = t_hours_per_type_per_gender$male)
#Result:
#Pearson's Chi-squared test
#data:  t_hours_per_type_per_gender$female and t_hours_per_type_per_gender$male
#X-squared = 72, df = 64, p-value = 0.2303

library(stats)
data <- matrix(c(t_hours_per_type_per_gender$female, t_hours_per_type_per_gender$male), nrow = 9, byrow = TRUE)
colnames(data) <- c("Women", "Men")
rownames(data) <- c(t_hours_per_type_per_gender$TypeTeaching)
chi_square_result <- chisq.test(data)
print(chi_square_result)
#Result:
#Pearson's Chi-squared test
#data:  data
#X-squared = 1105.9, df = 8, p-value < 2.2e-16
#Note that the two tests get different answers. This is because I think the first one could be incomplete. It doesn't specify rows by academic status.
```
.                                    |Male   |Female   |
-------------------------------------|-------|---------|
Other (admin)                        |  312  |   88    |
Other (course specific)              | 80.1  |   11.4  |
Other (mixed or unspecified teaching)|    6  |   130.  |
course convener                      |  251. |   216.  |
grading exams                        |  331. |   182.  |
grading thesis                       |  202. |   238.  |
lectures                             |  538. |   432.  |
seminars                             | 1036. |   861.  |
supervision                          |  561. |   609.  |
>Table that should in theory yield the p-value 2.2e-16
 
Conclusion:

The second test, which I believe to be the correct one, yields a really low p value. This suggests there's a very low chance at independence, and theres a strong relationship between teaching hours and gender. There is, in theory a gendered effect, and quite a significant one at that. (This could be completely false as this is the conclusion I, myself, have drawn from my limited knowledge.)
## Goal

To find out if gender and academic status matters for hours spent teaching.

## Tasks (written by supervisor)

- [x] Install RStudio
- [x] Do a chi-squared test in R
- [x] Prove understanding of chi-squared test
- [ ] Determine if tables in `[Bendfeldt & Söderström, unpublished]` are
  only descriptive or can be used to find a gendered effect.
  If a table shows a potential gendered effect, prove it is a
  significant effect
- [ ] At Uppsala University they know that males and females will never
  put in *exactly* the same amount of hours. Instead, they use a 
  rule-of-thumb of 40%-60% to say when the amounts of hours are
  equal enough. This is a different rule than when using statistics,
  where can get a significant effect of gender on teaching hours
  at 49% hours by males to 51% hours by female, **if and only if**
  the dataset is big enough. Are there examples when there is difference
  beyond the rule-of-thumb range and the statistics? When?
  How big should the data be for the 40%-60% rule-of-thumb to be valid?


## References (written by supervisor)

- `[Bendfeldt & Söderström, unpublished]`
  Fair division of labor? Examining access to prestigious
  teaching –formulating equitable and transparent policies




Code:
```R
#!/bin/env Rscript
setwd("analysis_soederstroem_20250408/")
t_persons <- readr::read_csv("teaching_person.csv")
t_hours <- readr::read_csv("teaching_hours.csv")
View(t_hours)

t_hours_per_gender <- t_hours |> 
  dplyr::select(GENDER, HOURS) |>
  dplyr::group_by(GENDER) |>
  dplyr::summarise(n_hours = sum(HOURS))

t_hours_per_gender
View(t_hours_per_gender)

chisq.test(x = c(8, 2), y = c(3, 7))
 

t_hours_per_type_per_gender <- t_hours |> 
  dplyr::select(GENDER, TypeTeaching, HOURS) |>
  dplyr::group_by(GENDER, TypeTeaching) |>
  dplyr::summarise(n_hours = sum(HOURS), .groups = "drop") |> 
  tidyr::pivot_wider(names_from = GENDER, values_from = n_hours)

t_hours_per_type_per_gender$female
t_hours_per_type_per_gender$

teaching_types <- unique(t_hours$TypeTeaching)
t_stats_per_gender_per_teaching_type <- tibble::tibble(
  teaching_type = teaching_types,
  p_value = NA,
  is_different = NA
)

for (i in seq_along(teaching_types)) {
  teaching_type <- teaching_types[i]
  male_hours <- t_hours |> 
    dplyr::select(GENDER, TypeTeaching, HOURS) |>
    dplyr::filter(TypeTeaching == teaching_type & GENDER == "male") |>
    dplyr::pull(HOURS)
  female_hours <- t_hours |> 
    dplyr::select(GENDER, TypeTeaching, HOURS) |>
    dplyr::filter(TypeTeaching == teaching_type & GENDER == "female") |>
    dplyr::pull(HOURS)
}

ggplot2::ggplot(
  t_hours |> dplyr::select(GENDER, TypeTeaching, HOURS),
  ggplot2::aes(x = HOURS, fill = GENDER)
) + ggplot2::geom_histogram(position = "identity", alpha = 0.5) + 
  ggplot2::facet_grid(TypeTeaching ~ ., ) +
  ggplot2::theme(strip.text.y.right = ggplot2::element_text(angle = 0))
ggplot2::ggsave(filename = "hours_per_gender_per_type.png", width = 7, height = 7)

library(stats)
data <- matrix(c(t_hours_per_type_per_gender$female, t_hours_per_type_per_gender$male), nrow = 9, byrow = TRUE)
colnames(data) <- c("Women", "Men")
rownames(data) <- c(t_hours_per_type_per_gender$TypeTeaching)
chi_square_result <- chisq.test(data)
print(chi_square_result)
#Will always output a p-value of 2.2e-16

chi_square_result[["observed"]]
#Result below:
#                                      Women      Men
#Other (admin)                         312.400   80.100
#Other (course specific)                 6.000  250.670
#Other (mixed or unspecified teaching) 330.865  201.500
#course convener                       538.330 1036.130
#grading exams                         561.170   88.000
#grading thesis                         11.375  130.500
#lectures                              216.500  182.500
#seminars                              238.250  431.500
#supervision                           861.490  608.875 -Matches data from analysis.
```
Since data from the observed chi_square_result matches the data from the analysis, I believe I can say that yes, there is a significant effect here. It is a reproducable result, and entering all the code will always yield the same p value.