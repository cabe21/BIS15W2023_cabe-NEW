---
title: "BIS 15L Midterm 2"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above.  

After the first 50 minutes, please upload your code (5 points). During the second 50 minutes, you may get help from each other- but no copy/paste. Upload the last version at the end of this time, but be sure to indicate it as final. If you finish early, you are free to leave.

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean! Use the tidyverse and pipes unless otherwise indicated. To receive full credit, all plots must have clearly labeled axes, a title, and consistent aesthetics. This exam is worth a total of 35 points. 

Please load the following libraries.

```r
library("tidyverse")
library("janitor")
library("naniar")
```

## Data
These data are from a study on surgical residents. The study was originally published by Sessier et al. “Operation Timing and 30-Day Mortality After Elective General Surgery”. Anesth Analg 2011; 113: 1423-8. The data were cleaned for instructional use by Amy S. Nowacki, “Surgery Timing Dataset”, TSHS Resources Portal (2016). Available at https://www.causeweb.org/tshs/surgery-timing/.

Descriptions of the variables and the study are included as pdf's in the data folder.  

Please run the following chunk to import the data.

```r
surgery <- read_csv("data/surgery.csv")
```

1. (2 points) Use the summary function(s) of your choice to explore the data and get an idea of its structure. Please also check for NA's.

```r
summary(surgery)
```

```
##    ahrq_ccs              age           gender              race          
##  Length:32001       Min.   : 1.00   Length:32001       Length:32001      
##  Class :character   1st Qu.:48.20   Class :character   Class :character  
##  Mode  :character   Median :58.60   Mode  :character   Mode  :character  
##                     Mean   :57.66                                        
##                     3rd Qu.:68.30                                        
##                     Max.   :90.00                                        
##                     NA's   :2                                            
##   asa_status             bmi        baseline_cancer    baseline_cvd      
##  Length:32001       Min.   : 2.15   Length:32001       Length:32001      
##  Class :character   1st Qu.:24.60   Class :character   Class :character  
##  Mode  :character   Median :28.19   Mode  :character   Mode  :character  
##                     Mean   :29.45                                        
##                     3rd Qu.:32.81                                        
##                     Max.   :92.59                                        
##                     NA's   :3290                                         
##  baseline_dementia  baseline_diabetes  baseline_digestive baseline_osteoart 
##  Length:32001       Length:32001       Length:32001       Length:32001      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  baseline_psych     baseline_pulmonary baseline_charlson mortality_rsi    
##  Length:32001       Length:32001       Min.   : 0.000    Min.   :-4.4000  
##  Class :character   Class :character   1st Qu.: 0.000    1st Qu.:-1.2400  
##  Mode  :character   Mode  :character   Median : 0.000    Median :-0.3000  
##                                        Mean   : 1.184    Mean   :-0.5316  
##                                        3rd Qu.: 2.000    3rd Qu.: 0.0000  
##                                        Max.   :13.000    Max.   : 4.8600  
##                                                                           
##  complication_rsi  ccsmort30rate      ccscomplicationrate      hour      
##  Min.   :-4.7200   Min.   :0.000000   Min.   :0.01612     Min.   : 6.00  
##  1st Qu.:-0.8400   1st Qu.:0.000789   1st Qu.:0.08198     1st Qu.: 7.65  
##  Median :-0.2700   Median :0.002764   Median :0.10937     Median : 9.65  
##  Mean   :-0.4091   Mean   :0.004312   Mean   :0.13325     Mean   :10.38  
##  3rd Qu.: 0.0000   3rd Qu.:0.007398   3rd Qu.:0.18337     3rd Qu.:12.72  
##  Max.   :13.3000   Max.   :0.016673   Max.   :0.46613     Max.   :19.00  
##                                                                          
##      dow               month            moonphase            mort30         
##  Length:32001       Length:32001       Length:32001       Length:32001      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  complication      
##  Length:32001      
##  Class :character  
##  Mode  :character  
##                    
##                    
##                    
## 
```

```r
naniar::miss_var_summary(surgery)
```

```
## # A tibble: 25 × 3
##    variable          n_miss pct_miss
##    <chr>              <int>    <dbl>
##  1 bmi                 3290 10.3    
##  2 race                 480  1.50   
##  3 asa_status             8  0.0250 
##  4 gender                 3  0.00937
##  5 age                    2  0.00625
##  6 ahrq_ccs               0  0      
##  7 baseline_cancer        0  0      
##  8 baseline_cvd           0  0      
##  9 baseline_dementia      0  0      
## 10 baseline_diabetes      0  0      
## # … with 15 more rows
```

```r
glimpse(surgery)
```

```
## Rows: 32,001
## Columns: 25
## $ ahrq_ccs            <chr> "<Other>", "<Other>", "<Other>", "<Other>", "<Othe…
## $ age                 <dbl> 67.8, 39.5, 56.5, 71.0, 56.3, 57.7, 56.6, 64.2, 66…
## $ gender              <chr> "M", "F", "F", "M", "M", "F", "M", "F", "M", "F", …
## $ race                <chr> "Caucasian", "Caucasian", "Caucasian", "Caucasian"…
## $ asa_status          <chr> "I-II", "I-II", "I-II", "III", "I-II", "I-II", "IV…
## $ bmi                 <dbl> 28.04, 37.85, 19.56, 32.22, 24.32, 40.30, 64.57, 4…
## $ baseline_cancer     <chr> "No", "No", "No", "No", "Yes", "No", "No", "No", "…
## $ baseline_cvd        <chr> "Yes", "Yes", "No", "Yes", "No", "Yes", "Yes", "Ye…
## $ baseline_dementia   <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ baseline_diabetes   <chr> "No", "No", "No", "No", "No", "No", "Yes", "No", "…
## $ baseline_digestive  <chr> "Yes", "No", "No", "No", "No", "No", "No", "No", "…
## $ baseline_osteoart   <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ baseline_psych      <chr> "No", "No", "No", "No", "No", "Yes", "No", "No", "…
## $ baseline_pulmonary  <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ baseline_charlson   <dbl> 0, 0, 0, 0, 0, 0, 2, 0, 1, 2, 0, 1, 0, 0, 0, 0, 0,…
## $ mortality_rsi       <dbl> -0.63, -0.63, -0.49, -1.38, 0.00, -0.77, -0.36, -0…
## $ complication_rsi    <dbl> -0.26, -0.26, 0.00, -1.15, 0.00, -0.84, -1.34, 0.0…
## $ ccsmort30rate       <dbl> 0.0042508, 0.0042508, 0.0042508, 0.0042508, 0.0042…
## $ ccscomplicationrate <dbl> 0.07226355, 0.07226355, 0.07226355, 0.07226355, 0.…
## $ hour                <dbl> 9.03, 18.48, 7.88, 8.80, 12.20, 7.67, 9.53, 7.52, …
## $ dow                 <chr> "Mon", "Wed", "Fri", "Wed", "Thu", "Thu", "Tue", "…
## $ month               <chr> "Nov", "Sep", "Aug", "Jun", "Aug", "Dec", "Apr", "…
## $ moonphase           <chr> "Full Moon", "New Moon", "Full Moon", "Last Quarte…
## $ mort30              <chr> "No", "No", "No", "No", "No", "No", "No", "No", "N…
## $ complication        <chr> "No", "No", "No", "No", "No", "No", "No", "Yes", "…
```


2. (3 points) Let's explore the participants in the study. Show a count of participants by race AND make a plot that visually represents your output.

```r
surgery %>%
  count(race)
```

```
## # A tibble: 4 × 2
##   race                 n
##   <chr>            <int>
## 1 African American  3790
## 2 Caucasian        26488
## 3 Other             1243
## 4 <NA>               480
```

```r
surgery%>% 
  ggplot(aes(x=race))+
  geom_bar()+
  labs(title = "Counts of Participants by Race",
       x = "Race",
       y = "Number of Patients")
```

![](midterm_2_files/figure-html/unnamed-chunk-7-1.png)<!-- -->


3. (2 points) What is the mean age of participants by gender? (hint: please provide a number for each) Since only three participants do not have gender indicated, remove these participants from the data.

```r
surgery %>% 
  group_by(gender) %>% 
  summarise(mean_age = mean(age, na.rm= T)) %>% 
  filter( gender != "na")
```

```
## # A tibble: 2 × 2
##   gender mean_age
##   <chr>     <dbl>
## 1 F          56.7
## 2 M          58.8
```
4. (3 points) Make a plot that shows the range of age associated with gender.

```r
surgery %>% 
  select(gender, age) %>% 
  filter(gender != "na") %>% 
  ggplot(aes(x=gender, y=age))+geom_boxplot()
```

```
## Warning: Removed 2 rows containing non-finite values (`stat_boxplot()`).
```

![](midterm_2_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
labs(title = "Distribution of Age by Gender",
       x = "Gender",
       y = "Ager of Patients")
```

```
## $x
## [1] "Gender"
## 
## $y
## [1] "Ager of Patients"
## 
## $title
## [1] "Distribution of Age by Gender"
## 
## attr(,"class")
## [1] "labels"
```

comparison <- surgery %>% 
  group_by(gender) %>% 
  summarise(mean_age = mean(age, na.rm= T), median_age = median(age, na.rm= T), min_age= min(age, na.rm= T), max_age = max(age, na.rm= T)) %>% 
  filter( gender != "na")
comparison


5. (2 points) How healthy are the participants? The variable `asa_status` is an evaluation of patient physical status prior to surgery. Lower numbers indicate fewer comorbidities (presence of two or more diseases or medical conditions in a patient). Make a plot that compares the number of `asa_status` I-II, III, and IV-V.

```r
surgery%>% 
  filter(asa_status != "NA") %>% 
  ggplot(aes(x=asa_status))+
  geom_bar()+
  labs(title = "Counts of asa_status' Among Patients",
       x = "Asa_status (by category)",
       y = "Number of Statuses Among Patients")
```

![](midterm_2_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

6. (3 points) Create a plot that displays the distribution of body mass index for each `asa_status` as a probability distribution- not a histogram. (hint: use faceting!)

```r
surgery %>% 
  filter(asa_status != "NA") %>% 
  ggplot(aes(x=bmi))+ 
  geom_density(fill="deepskyblue4", alpha  =0.8, color = "black") + 
  facet_wrap(~asa_status, ncol=3)+
  labs(title = "BMI Distrution of Patients per asa_status",
       x = "BMI",
       y = "Percentage of Patients")
```

```
## Warning: Removed 3289 rows containing non-finite values (`stat_density()`).
```

![](midterm_2_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

The variable `ccsmort30rate` is a measure of the overall 30-day mortality rate associated with each type of operation. The variable `ccscomplicationrate` is a measure of the 30-day in-hospital complication rate. The variable `ahrq_ccs` lists each type of operation.  

7. (4 points) What are the 5 procedures associated with highest risk of 30-day mortality AND how do they compare with the 5 procedures with highest risk of complication? (hint: no need for a plot here)

```r
surgery %>% 
  group_by(ahrq_ccs) %>% 
summarise(average_30daymortality = mean(ccsmort30rate)) %>% 
arrange(desc(average_30daymortality)) %>% 
top_n(5, ahrq_ccs)
```

```
## # A tibble: 5 × 2
##   ahrq_ccs                                                         average_30d…¹
##   <chr>                                                                    <dbl>
## 1 Small bowel resection                                                 0.0129  
## 2 Spinal fusion                                                         0.00742 
## 3 Repair of cystocele and rectocele; obliteration of vaginal vault      0.00210 
## 4 Transurethral resection of prostate (TURP)                            0.00141 
## 5 Thyroidectomy; partial or complete                                    0.000672
## # … with abbreviated variable name ¹​average_30daymortality
```


```r
surgery %>% 
  group_by(ahrq_ccs) %>% 
summarise(average_complicationrate = mean(ccscomplicationrate)) %>% 
arrange(desc(average_complicationrate)) %>% 
  top_n(5, ahrq_ccs)
```

```
## # A tibble: 5 × 2
##   ahrq_ccs                                                         average_com…¹
##   <chr>                                                                    <dbl>
## 1 Small bowel resection                                                   0.466 
## 2 Spinal fusion                                                           0.183 
## 3 Repair of cystocele and rectocele; obliteration of vaginal vault        0.0840
## 4 Transurethral resection of prostate (TURP)                              0.0268
## 5 Thyroidectomy; partial or complete                                      0.0161
## # … with abbreviated variable name ¹​average_complicationrate
```


surgery %>% 
    pivot_longer(baseline_cancer : baseline_pulmonary,
               names_to = "procedure", 
               names_prefix = "baseline_",
               values_to="had/didn't_procedure") %>% 
               
8. (3 points) Make a plot that compares the `ccsmort30rate` for all listed `ahrq_ccs` procedures.

```r
surgery %>% 
  ggplot(aes(x=ahrq_ccs, y=ccsmort30rate))+ 
  geom_col() + 
  coord_flip()+
  labs(title = "30 Day Mortality Rate for Procedures",
       x = "Procedure",
       y = "30 Day Mortality Rate (%)")
```

![](midterm_2_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

9. (4 points) When is the best month to have surgery? Make a chart that shows the 30-day mortality and complications for the patients by month. `mort30` is the variable that shows whether or not a patient survived 30 days post-operation.

```r
surgery %>% 
  mutate(death= case_when(mort30 == "Yes"~ 1, mort30 == "No"~ 0)) %>% 
  group_by(month) %>% 
  summarise(total_deaths= sum(death))
```

```
## # A tibble: 12 × 2
##    month total_deaths
##    <chr>        <dbl>
##  1 Apr             12
##  2 Aug              9
##  3 Dec              4
##  4 Feb             17
##  5 Jan             19
##  6 Jul             12
##  7 Jun             14
##  8 Mar             12
##  9 May             10
## 10 Nov              5
## 11 Oct              8
## 12 Sep             16
```
##Jan is the month with the most death and Decmeber has the least death. 
10. (4 points) Make a plot that visualizes the chart from question #9. Make sure that the months are on the x-axis. Do a search online and figure out how to order the months Jan-Dec.

```r
surgery %>% 
  mutate(death= case_when(mort30 == "Yes"~ 1, mort30 == "No"~ 0)) %>% 
  group_by(month) %>% 
  summarise(total_deaths= sum(death)) %>% 
  ggplot(aes(x = month, y=total_deaths))+
  geom_col()+
   labs(title = "Number of Deaths Per Month",
       x = "Month",
       y = "Number of Deaths")
```

![](midterm_2_files/figure-html/unnamed-chunk-16-1.png)<!-- -->



Please provide the names of the students you have worked with with during the exam:

Please be 100% sure your exam is saved, knitted, and pushed to your github repository. No need to submit a link on canvas, we will find your exam in your repository.
