<<<<<<< HEAD
library(knitr)
==============

rmarkdown::render("ExploratoryAnalysis.Rmd")
============================================

Data preprocessing & Exploratory analysis of Titanic dataset
============================================================

library(ggplot2) library(dplyr) \# Data Preprocessing

Read Titanic train dataset into a R dataframe
=============================================

Titanic\_Train&lt;-read.csv('train.csv')

Titanic\_Train=Titanic\_Train\[c(-1,-4,-9 )\]

See the variable sturcture of Train dataframe
=============================================

str (Titanic\_Train)

Since survived is a categorical variable it is convterted to a factor
=====================================================================

Titanic\_Train*S**u**r**v**i**v**e**d* &lt; −*f**a**c**t**o**r*(*T**i**t**a**n**i**c*<sub>*T*</sub>*r**a**i**n*Survived,
levels = c('0','1'))
Titanic\_Train*P**c**l**a**s**s* &lt; −*f**a**c**t**o**r*(*T**i**t**a**n**i**c*<sub>*T*</sub>*r**a**i**n*Pclass,
levels = c('1','2','3'))

Find number of missing vlaues in Age vector
===========================================

summary(Titanic\_Train)
=======================

To replace the 177 missing values in Age Calculate mean age for both the genders
================================================================================

meanAgeMale&lt;- Titanic\_Train %&gt;% group\_by(Sex)%&gt;%
summarise(meanAge = mean(Age, na.rm = TRUE))%&gt;% filter(Sex ==
'male');

meanMale&lt;-meanAgeMale\[1,2\] meanMale &lt;-round(meanMale)

meanAgeFemale&lt;- Titanic\_Train %&gt;% group\_by(Sex)%&gt;%
summarise(meanAge = mean(Age, na.rm = TRUE))%&gt;% filter(Sex ==
'female');

meanFemale&lt;-meanAgeFemale\[1,2\] meanFemale &lt;-round(meanFemale)

Replace missing ages with average ages of respective genders
============================================================

Titanic\_Train*A**g**e*\[*i**s*.*n**a*(*T**i**t**a**n**i**c*<sub>*T*</sub>*r**a**i**n*Age)
&
Titanic\_Train*S**e**x* = =′*m**a**l**e*′\] &lt; −*M**e**a**n**M**a**l**e**T**i**t**a**n**i**c*<sub>*T*</sub>*r**a**i**n*Age\[is.na(Titanic\_Train$Age) & Titanic\_Train$Sex
== 'female'\]&lt;- MeanFemale

summary(Titanic\_Train$Age) \#\# R Markdown

    load(file="Titanic_Train.RData")
    library(ggplot2)
    ggplot(data = Titanic_Train, aes(x="", y=Age,color=Age)) + 
      geom_jitter()+
      geom_boxplot( alpha = 0.7, color = "Red", outlier.color = NA) 

![](ExploratoryAnalysis_files/figure-markdown_strict/unnamed-chunk-1-1.png)

    #outlier.color = NA removes the duplicate datapoints shown as a part of the boxplot which was already covered in jitters

    # Use histogram to see frequency disribution of Age
    ggplot(data = Titanic_Train, aes(Age)) + 
                  geom_histogram(binwidth = 2, aes(fill = Survived), color="black") +
                  geom_text(stat="bin", aes(label = ..count.., size = 3))+
                  labs(title = "Age Histogram")+
                  labs(x = "Age", y = "Count")

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](ExploratoryAnalysis_files/figure-markdown_strict/unnamed-chunk-1-2.png)

    # conclusion : The age histogtam is bi modal, almost eveyone in the first group age between 0- 10 survived indicating children were given preferrance, more people from the age group 20-40 couldnot survive

    ggplot(data = Titanic_Train, aes(Sex)) + 
                  geom_bar(aes(fill = Survived), color="Black") 

![](ExploratoryAnalysis_files/figure-markdown_strict/unnamed-chunk-1-3.png)

    # result : MOre percantage of female passengers survived as compared to male passenger
    ggplot(data = Titanic_Train, aes(Pclass)) + 
                  geom_bar(aes(fill = Survived), color="Black")

![](ExploratoryAnalysis_files/figure-markdown_strict/unnamed-chunk-1-4.png)

    # to see if there is any pattern in place of embarkment
    ggplot(data = Titanic_Train, aes(Embarked)) + 
                  geom_bar(aes(fill = Survived), color="Black")

![](ExploratoryAnalysis_files/figure-markdown_strict/unnamed-chunk-1-5.png)

    # To see which class which gender travelled?:P
    ggplot(data = Titanic_Train, aes(Pclass)) + 
                  geom_bar(aes(fill = Sex), color = "black")

![](ExploratoryAnalysis_files/figure-markdown_strict/unnamed-chunk-1-6.png)
=======
Exploratory Analysis
================
Padmaja Gharge
8 March 2018

library(knitr)
==============

rmarkdown::render("ExploratoryAnalysis.Rmd")
============================================

Data preprocessing & Exploratory analysis of Titanic dataset
============================================================

library(ggplot2) library(dplyr) \# Data Preprocessing

Read Titanic train dataset into a R dataframe
=============================================

Titanic\_Train&lt;-read.csv('train.csv')

Titanic\_Train=Titanic\_Train\[c(-1,-4,-9 )\]

See the variable sturcture of Train dataframe
=============================================

str (Titanic\_Train)

Since survived is a categorical variable it is convterted to a factor
=====================================================================

Titanic\_Train*S**u**r**v**i**v**e**d* &lt; −*f**a**c**t**o**r*(*T**i**t**a**n**i**c*<sub>*T*</sub>*r**a**i**n*Survived, levels = c('0','1')) Titanic\_Train*P**c**l**a**s**s* &lt; −*f**a**c**t**o**r*(*T**i**t**a**n**i**c*<sub>*T*</sub>*r**a**i**n*Pclass, levels = c('1','2','3'))

Find number of missing vlaues in Age vector
===========================================

summary(Titanic\_Train)
=======================

To replace the 177 missing values in Age Calculate mean age for both the genders
================================================================================

meanAgeMale&lt;- Titanic\_Train %&gt;% group\_by(Sex)%&gt;% summarise(meanAge = mean(Age, na.rm = TRUE))%&gt;% filter(Sex == 'male');

meanMale&lt;-meanAgeMale\[1,2\] meanMale &lt;-round(meanMale)

meanAgeFemale&lt;- Titanic\_Train %&gt;% group\_by(Sex)%&gt;% summarise(meanAge = mean(Age, na.rm = TRUE))%&gt;% filter(Sex == 'female');

meanFemale&lt;-meanAgeFemale\[1,2\] meanFemale &lt;-round(meanFemale)

Replace missing ages with average ages of respective genders
============================================================

Titanic\_Train*A**g**e*\[*i**s*.*n**a*(*T**i**t**a**n**i**c*<sub>*T*</sub>*r**a**i**n*Age) & Titanic\_Train*S**e**x* = =′*m**a**l**e*′\] &lt; −*M**e**a**n**M**a**l**e**T**i**t**a**n**i**c*<sub>*T*</sub>*r**a**i**n*Age\[is.na(Titanic\_Train$Age) & Titanic\_Train$Sex == 'female'\]&lt;- MeanFemale

summary(Titanic\_Train$Age) \#\# R Markdown

``` r
load(file="Titanic_Train.RData")
library(ggplot2)
ggplot(data = Titanic_Train, aes(x="", y=Age,color=Age)) + 
  geom_jitter()+
  geom_boxplot( alpha = 0.7, color = "Red", outlier.color = NA) 
```

![](ExploratoryAnalysis_files/figure-markdown_github/unnamed-chunk-1-1.png)

``` r
#outlier.color = NA removes the duplicate datapoints shown as a part of the boxplot which was already covered in jitters

# Use histogram to see frequency disribution of Age
ggplot(data = Titanic_Train, aes(Age)) + 
              geom_histogram(binwidth = 2, aes(fill = Survived), color="black") +
              labs(title = "Age Histogram")+
              labs(x = "Age", y = "Count")
```

![](ExploratoryAnalysis_files/figure-markdown_github/unnamed-chunk-1-2.png)

``` r
# conclusion : The age histogtam is bi modal, almost eveyone in the first group age between 0- 10 survived indicating children were given preferrance, more people from the age group 20-40 couldnot survive

ggplot(data = Titanic_Train, aes(Sex)) + 
              geom_bar(aes(fill = Survived), color="Black") 
```

![](ExploratoryAnalysis_files/figure-markdown_github/unnamed-chunk-1-3.png)

``` r
# result : MOre percantage of female passengers survived as compared to male passenger
ggplot(data = Titanic_Train, aes(Pclass)) + 
              geom_bar(aes(fill = Survived), color="Black")
```

![](ExploratoryAnalysis_files/figure-markdown_github/unnamed-chunk-1-4.png)

``` r
# to see if there is any pattern in place of embarkment
ggplot(data = Titanic_Train, aes(Embarked)) + 
              geom_bar(aes(fill = Survived), color="Black")
```

![](ExploratoryAnalysis_files/figure-markdown_github/unnamed-chunk-1-5.png)

``` r
# To see which class which gender travelled?:P
ggplot(data = Titanic_Train, aes(Pclass)) + 
              geom_bar(aes(fill = Sex), color = "black")
```

![](ExploratoryAnalysis_files/figure-markdown_github/unnamed-chunk-1-6.png)
>>>>>>> 4438f8365a5365861ef029275091d730059a76bb
