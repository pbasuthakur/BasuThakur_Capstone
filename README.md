# BasuThakur_Capstone
Capstone Project-2020
Poulami Basu Thakur
4/27/2020
R Markdown
This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see http://rmarkdown.rstudio.com.
When you click the Knit button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:
library(readxl)
library(tidyverse)
## Warning: package 'tidyverse' was built under R version 3.6.3
## -- Attaching packages ----------------------------------------------------------------------- tidyverse 1.3.0 --
## v ggplot2 3.3.0     v purrr   0.3.4
## v tibble  3.0.1     v dplyr   0.8.5
## v tidyr   1.0.2     v stringr 1.4.0
## v readr   1.3.1     v forcats 0.5.0
## Warning: package 'ggplot2' was built under R version 3.6.3
## Warning: package 'tibble' was built under R version 3.6.3
## Warning: package 'tidyr' was built under R version 3.6.3
## Warning: package 'purrr' was built under R version 3.6.3
## Warning: package 'dplyr' was built under R version 3.6.3
## Warning: package 'forcats' was built under R version 3.6.3
## -- Conflicts -------------------------------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
library(minpack.lm)
library(nlfitr)
library(broom)
## Warning: package 'broom' was built under R version 3.6.3
library(knitr)
## Warning: package 'knitr' was built under R version 3.6.3
library(ez)
## Registered S3 methods overwritten by 'lme4':
##   method                          from
##   cooks.distance.influence.merMod car 
##   influence.merMod                car 
##   dfbeta.influence.merMod         car 
##   dfbetas.influence.merMod        car
library(cowplot)
## 
## ********************************************************
## Note: As of version 1.0.0, cowplot does not change the
##   default ggplot2 theme anymore. To recover the previous
##   behavior, execute:
##   theme_set(theme_cowplot())
## ********************************************************
library(viridis)
## Loading required package: viridisLite
library(Hmisc)
## Warning: package 'Hmisc' was built under R version 3.6.3
## Loading required package: lattice
## Loading required package: survival
## Loading required package: Formula
## 
## Attaching package: 'Hmisc'
## The following objects are masked from 'package:dplyr':
## 
##     src, summarize
## The following objects are masked from 'package:base':
## 
##     format.pval, units
library(kableExtra)
## Warning: package 'kableExtra' was built under R version 3.6.3
## 
## Attaching package: 'kableExtra'
## The following object is masked from 'package:dplyr':
## 
##     group_rows
Task 1: Brief background and significance of problem
Lactobacillus reuteri, a Gram-positive probiotic bacterium, is a natural inhabitant of the gastrointestinal tract of vertebrates. According to the WHO, probiotics are live microorganisms which when consumed in adequate amounts, are known to confer health benefits on the host. L. reuteri is known to have immunomodulatory and other beneficial effects on its host, such as, fighting infection causing bacteria in the gut. Inflammation in the gut is characterized by the production of reactive oxygen species, such as, hydrogen peroxide (H2O2), as well as reactive chlorine species such as hypochlorous acid (HOCl). My study aims to understand if L. reuteri responds diffrently to the inflammatory oxidants, HOCl and H2O2; if so, how does L. reuteri combat the deleterious effects of these potent redox molecules.
Task 2:
H2O2 and HOCl resistance pathways have been well studied in E. coli; however, little is known about how commensal and/ or probiotic bacteria respond to inflammatory oxidants. Therefore, to test how this bacterium responds to the inflammatory stressors, HOCl and H2O2, wild type L. reuteri cells were grown in MRS (de Man Rogosa Sharpe) medium with or without a sublethal dose of HOCl and H2O2. A sublethal dose serves to retard bacterial growth over a period of time without killing it.
Task 3:
If L. reuteri is able to neutralize HOCl and H2O2 over time, then it should grow slower in the presence of the inflammatory molecules as compared to growth in medium without added HOCl and H2O2.
Task 4:
The dependent variable used to test this prediction in item #3 is the concentration of bacterial cells in the growth medium, measured as absorbance at 600 nm (A600) on a UV spectrophotometer. The predictor variables are discrete factors with three levels, no stress, HOCl stress and H2O2 stress.
Task 5:
Null hypothesis, H0: µno_treatment = µHOCl_treatment = uH2O2_treatment. There is no difference in growth of L. reuteri in medium that is supplemented with or without inflammatory oxidants, HOCl or H2O2.
Alternate hypothesis, Ha: µno_treatment ≠ µHOCl_treatment ≠ µH2O2_treatment. The growth of L. reuteri is altered in medium supplemented with HOCl or H2O2, as compared to growth in medium not supplemented with HOCl or H2O2.
The null statistical hypothesis is that the variance associated with the different levels of stress treatment is less than or equal to the residual variance. Therefore, the alternate hypothesis is the variance associated with stress treatment is greater than residual variance.
Task 6:
I used the one way completely randomized ANOVA statistical test to test the hypothesis in item #5. This test is appropriate for the experimental hypothesis because the predictor variable is a factor with three discrete levels, and the outcome/ response variable is continuous and measured.
Task 7:
I used the ezANOVA function and omnibus analysis to perform the one way completely randomized ANOVA. The outcome variable (absorbance at 600 nm or A600) is in measured units and at equal interval scales. The predictor variable is a factor (stress treatment) with three levels (no stress, HOCl stress and H2O2 stress). The measurements are not intrinsically related. There are three hypotheses in one experiment and 15 subjects.There is a family-wise ~5% type1 error.
Task 8:
data <- read.csv("datasets/bacterialstress.csv"); data
##     A600 Treatment
## 1  4.020 No stress
## 2  3.890 No stress
## 3  3.840 No stress
## 4  4.120 No stress
## 5  4.470 No stress
## 6  2.020      HOCl
## 7  1.590      HOCl
## 8  1.890      HOCl
## 9  2.300      HOCl
## 10 2.100      HOCl
## 11 0.780      H2O2
## 12 0.990      H2O2
## 13 0.720      H2O2
## 14 0.689      H2O2
## 15 0.900      H2O2
str(data)
## 'data.frame':    15 obs. of  2 variables:
##  $ A600     : num  4.02 3.89 3.84 4.12 4.47 2.02 1.59 1.89 2.3 2.1 ...
##  $ Treatment: Factor w/ 3 levels "H2O2","HOCl",..: 3 3 3 3 3 2 2 2 2 2 ...
data1 <- data %>%
  group_by(Treatment) %>%
  summarise(
    mean= mean(A600),
    median=median(A600),
    sd= sd(A600),
    n = n(),
    var=var(A600)
    ); data1
## # A tibble: 3 x 6
##   Treatment  mean median    sd     n    var
##   <fct>     <dbl>  <dbl> <dbl> <int>  <dbl>
## 1 H2O2      0.816   0.78 0.126     5 0.0160
## 2 HOCl      1.98    2.02 0.264     5 0.0696
## 3 No stress 4.07    4.02 0.250     5 0.0626
knitr::kable(data1, caption="Descriptive Statistics for the Bacterial Stress Treatments Dataset.")
Descriptive Statistics for the Bacterial Stress Treatments Dataset. 
Treatment 
mean 
median 
sd 
n 
var 
H2O2 
0.8158 
0.78 
0.1264642 
5 
0.0159932 
HOCl 
1.9800 
2.02 
0.2639129 
5 
0.0696500 
No stress 
4.0680 
4.02 
0.2501400 
5 
0.0625700 
ggplot(data, aes(Treatment, A600, color=Treatment))+
  geom_point(width = 0.1, size=4, alpha=0.5) +
  xlab("Stress Treatment") +
  ylab("Bacterial Absorbance (A600)") +
  ggtitle("Effects of Inflammatory Stress Treatment on Bacterial Growth in L. reuteri.") +
  geom_boxplot()

data$ID <- as.factor(1:nrow(data))

my.ezaov <- ezANOVA(
            data = data, 
            wid = ID, 
            dv = A600, 
            between = Treatment,
            type = 2, 
            return_aov = T, 
            detailed = T)

my.ezaov
## $ANOVA
##      Effect DFn DFd      SSn       SSd        F            p p<.05       ges
## 1 Treatment   2  12 27.15318 0.5928528 274.8053 9.516461e-11     * 0.9786329
## 
## $`Levene's Test for Homogeneity of Variance`
##   DFn DFd        SSn       SSd         F         p p<.05
## 1   2  12 0.02266413 0.2553088 0.5326287 0.6003156      
## 
## $aov
## Call:
##    aov(formula = formula(aov_formula), data = data)
## 
## Terms:
##                 Treatment Residuals
## Sum of Squares  27.153184  0.592853
## Deg. of Freedom         2        12
## 
## Residual standard error: 0.222271
## Estimated effects may be unbalanced
Task 9:
b = 100 #expected basal outcome value
a = 1.5 #expected fold-to-basal effect of our positive control
f = 1.25 #minimal scientifically relevant fold-to-basal effect of our treatment
sd = 10 #expected standard deviation of Outcome variable
n = 10 # number of independent replicates per group
sims = 100 #number of Monte Carlo simulations to run. 

CRdataMaker <- function(n, b, a, f, sd) { 
  
  
  a1 <- rnorm(n, b, sd) #basal or negative ctrl
  no_stress <- rnorm(n, (b*a), sd) 
  HOCl_stress <- rnorm(n, (b*f), sd) #treatment effect
  H2O2_stress <- rnorm(n, (b*f), sd) #treatment effect
    
    Outcome <- c(no_stress, HOCl_stress, H2O2_stress)
    Predictor <- c(rep(c("no_stress", "HOCl_stress", "H2O2_stress"), each = n))
    ID <- as.factor(c(1:length(Predictor)))
    df <-data.frame(ID, Predictor, Outcome)
    }

dat <- CRdataMaker(n,b,a,f,sd)

ggplot(dat, aes(Predictor, Outcome))+
  geom_jitter(width=0.1,size = 4, alpha=0.5, color="blue") +
  labs(y="Bacterial cell absorbance, A600", x="Stress treatment") +
  ggtitle("Effects of Inflammatory Stress Treatment on Bacterial Growth in L. reuteri.")

pval <- replicate(
  sims, {
 
    sample.df <- CRdataMaker(n, b, a, f, sd)
    
    sim.ezaov <- ezANOVA(
            data = sample.df, 
            wid = ID,
            dv = Outcome,
            between = Predictor,
            type = 2
            )
  
  pval <- sim.ezaov$ANOVA[1,5]
    
    }
  )

pwr.pct <- sum(pval<0.05)/sims*100
paste(pwr.pct, sep="", "% power. Change 'n' in your initializer for higher or lower power.")
## [1] "100% power. Change 'n' in your initializer for higher or lower power."
ggplot(data.frame(pval))+
  geom_histogram(aes(pval), color="#d28e00")+
  labs(x="p-value")
