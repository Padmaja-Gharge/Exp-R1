Before we start analyzing the Titanic Dataset we need to take care of
missing values in the dataset. The Age attribute has highest number of
missing values which could be replaced by the mean age. But since we
have gender information also in the dataset it is better to calculate
mean age for each gender and replace the respective mean ages in place
of missing age.

> library(dplyr)

> Titanic\_Train&lt;-read.csv('train.csv')
> Titanic\_Test&lt;-read.csv('test.csv')

Mean age for men:

    library(dplyr)

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    Titanic_Train<-read.csv('train.csv')
    Titanic_Test<-read.csv('test.csv')

    load(file="Titanic_Train.RData")

      meanAgeMale<- Titanic_Train %>% 
      group_by(Sex)%>% 
      summarise(meanAge = mean(Age, na.rm = TRUE))%>%
      filter(Sex == 'male');

    meanMale<-round(as.double(meanAgeMale[1,2]))

Mean age for females

    meanAgeFemale<- Titanic_Train %>% 
      group_by(Sex)%>% 
      summarise(meanAge = mean(Age, na.rm = TRUE))%>%
      filter(Sex == 'female');

    meanFemale<-round(as.double(meanAgeFemale[1,2]))

Replace missing ages with respective average ages based on gender

    Titanic_Train$Age[is.na(Titanic_Train$Age) & Titanic_Train$Sex == 'male'] <-meanMale
    Titanic_Train$Age[is.na(Titanic_Train$Age) & Titanic_Train$Sex == 'female']<- meanFemale

Titanic*A**g**e* &lt; −*r**o**u**n**d*(*T**i**t**a**n**i**c*Age)

Convert character and numerical vectors into factors

    Titanic_Train$Survived<-factor(Titanic_Train$Survived)

    # See the columns available in dataset
    colnames(Titanic_Train)

    ## [1] "Survived" "Pclass"   "Sex"      "Age"      "SibSp"    "Parch"   
    ## [7] "Fare"     "Cabin"    "Embarked"

    # Removing unwanted columns PassengerID, Name and Ticket Number

    Titanic_Train=Titanic_Train[c(-1,-4,-9 )]


    sapply(Titanic_Test, class)

    ## PassengerId      Pclass        Name         Sex         Age       SibSp 
    ##   "integer"   "integer"    "factor"    "factor"   "numeric"   "integer" 
    ##       Parch      Ticket        Fare       Cabin    Embarked 
    ##   "integer"    "factor"   "numeric"    "factor"    "factor"

    Titanic_Test=Titanic_Test[,-c(3,8 )]


    # Calculate mean age for men for test dataset

    meanAgeMale<- Titanic_Test %>% 
      group_by(Sex)%>% 
      summarise(meanAge = mean(Age, na.rm = TRUE))%>%
      filter(Sex == 'male');

    meanMale<-meanAgeMale[1,2]
    meanMale <-round(meanMale)

    #Calculate mean age for females

    meanAgeFemale<- Titanic_Test %>% 
      group_by(Sex)%>% 
      summarise(meanAge = mean(Age, na.rm = TRUE))%>%
      filter(Sex == 'female');

    meanFemale<-meanAgeFemale[1,2]
    meanFemale <-round(meanFemale)


    # Replace missing ages with respective average ages based on gender

    Titanic_Test$Age[is.na(Titanic_Test$Age) & Titanic_Test$Sex == 'male'] <-meanMale
    Titanic_Test$Age[is.na(Titanic_Test$Age) & Titanic_Test$Sex == 'female']<- meanFemale

    # Matching levels of fators in Train and Test datasets

    Titanic_Test$Sex <- factor(Titanic_Test$Sex, 
                               levels =levels(Titanic_Train$Sex))
    Titanic_Test$Cabin <- factor(Titanic_Test$Cabin, 
                                 levels =levels(Titanic_Train$Cabin))
    Titanic_Test$Embarked <- factor(Titanic_Test$Embarked, 
                                   levels =levels(Titanic_Train$Embarked))



    str(Titanic_Train)

    ## 'data.frame':    891 obs. of  6 variables:
    ##  $ Pclass: Factor w/ 3 levels "1","2","3": 3 1 3 1 3 3 1 3 3 2 ...
    ##  $ Sex   : Factor w/ 2 levels "female","male": 2 1 1 1 2 2 2 2 1 1 ...
    ##  $ SibSp : int  1 1 0 1 0 0 0 3 0 1 ...
    ##  $ Parch : int  0 0 0 0 0 0 0 1 2 0 ...
    ##  $ Fare  : num  7.25 71.28 7.92 53.1 8.05 ...
    ##  $ Cabin : Factor w/ 148 levels "","A10","A14",..: 1 83 1 57 1 1 131 1 1 1 ...

    Titanic_Test$Fare <- ifelse (is.na(Titanic_Test$Fare),
                                 mean(Titanic_Test$Fare,na.rm = TRUE),
                                 Titanic_Test$Fare)

Predtictive models:

1.  Random forest

#### \`\`\`{r, Titanic\_Train, echo = TRUE, include= TRUE}

library(randomForest) set.seed(123) str(Titanic\_Train) M1
&lt;-randomForest( as.factor(Survived) ~ Pclass + Sex + Age + SibSp +
Parch + Embarked, data = Titanic\_Train, ntree= 500) summary(M1)
varImpPlot(M1)

sapply(Titanic\_Train, class) sapply(Titanic\_Test, class)

Titanic\_Test$Survived1 &lt;- predict(M1, newdata =
Titanic\_Test\[,-c(1)\])

submission1 &lt;- Titanic\_Test\[,c("PassengerId","Survived1")\]

write.table(submission1, file = "submission\_pg.csv", col.names = TRUE,
row.names = FALSE, sep = ",")

str(submission1) \#\#\#\#\`\`\`

1.  Logistic Regression

#### \`\`\` {r}

M2 &lt;- glm (Survived ~ Pclass + Sex + Age + SibSp + Parch + Fare +
Embarked, family = binomial(link = 'logit'), data = Titanic\_Train)

summary(M2)

anova(M2, test="Chisq")

Titanic\_Test$Survived2&lt;- predict(M2, newdata =
Titanic\_Test\[,-c(1)\]) submission2 &lt;-
Titanic\_Test\[,c("PassengerId","Survived2")\]

#### \`\`\`

1.  Naive Bayes \#\#\#\#\`\`\` {r}

library(e1071)

M3 &lt;- naiveBayes(Survived ~ Pclass + Sex + Age + SibSp + Parch + Fare
+ &gt; &gt; &gt; &gt; Embarked, data = Titanic\_Train)

class(M3) summary(M3)

Titanic\_Test$Survived3&lt;- predict(M3, newdata =
Titanic\_Test\[,-c(1)\]) submission3 &lt;-
Titanic\_Test\[,c("PassengerId","Survived3")\]

#### \`\`\`

1.  Decision Tree

#### \`\`\` {r}

library(rattle) library(rpart)

Tree\_model &lt;- rpart(Survived ~ Pclass + Sex + Age + SibSp + Parch +
Fare+ Embarked, data = Titanic\_Train,control =rpart.control(minsplit =
10, cp = 0.00))

fancyRpartPlot(Tree\_model, sub="decision tree")

Titanic\_Test$Survived4=predict(Tree\_model,
Titanic\_Test\[,-c(1)\],type="class")

#### \`\`\`
