Relationship between Exercise Addiction Risk and Social Interaction is
Moderated by Depression and Eating Disorder
================
Chia-Chuan Yu, Ph.D.
2026-09-30

# Introduction

This is the 1st study in my dissertation, supervised by Darla Castelli,
Ph.D. We investigated the associations between exercise addiction (EA)
and three domains of social interaction (i.e., social support\[ss\],
perceived discrimination\[dis\], fear of missing out \[FoMO\], and the
importance of social reward\[sr\]) among young adults in Austin, TX.
Furthermore, we examined whther these relationships are moderated by the
symptoms of depression and eating disorder, which are the two common
comorbidities of EA.

# Data imputation

This step is skipped because I’ve already done the imputation, which
took a long time to run.

``` r
# df <- read_excel(path = "/Users/chia-chuanyu/Desktop/dissertationData/study1/study1_organizedFinal.xlsx",
#                  sheet = "organized_forImputation")
# column_names <- c('eai1', 'eai2', 'eai3', 'eai4', 'eai5', 'eai6',
#               'idea81', 'idea82', 'idea83', 'idea84', 'idea85', 'idea86', 'idea87', 'idea88',
#               'ipaqsf_vigMET', 'ipaqsf_modMET', 'ipaqsf_walkMET', 'ipaqsf_sit',
#               'edeqs1', 'edeqs2', 'edeqs3', 'edeqs4', 'edeqs5', 'edeqs6', 'edeqs7', 'edeqs8', 'edeqs9', 'edeqs10', 'edeqs11', 'edeqs12',
#               'phq1', 'phq2', 'phq3', 'phq4', 'phq5', 'phq6', 'phq7', 'phq8', 'phq10',
#               'mosss1', 'mosss2', 'mosss3', 'mosss4', 'mosss5', 'mosss6', 'mosss7', 'mosss8', 'mosss9', 'mosss10', 'mosss11', 'mosss12',
#               'mosss13', 'mosss14', 'mosss15', 'mosss16', 'mosss17', 'mosss18', 'mosss19',
#               'srq1', 'srq2', 'srq3', 'srq4', 'srq5', 'srq6', 'srq7', 'srq8', 'srq9', 'srq10', 'srq11', 'srq12', 'srq13', 'srq14', 'srq15',
#               'srq16', 'srq17', 'srq18', 'srq19', 'srq20', 'srq21', 'srq22', 'srq23',
#               'fomos1', 'fomos2', 'fomos3', 'fomos4', 'fomos5', 'fomos6', 'fomos7', 'fomos8', 'fomos9', 'fomos10',
#               'everyDiscrimiS1a','everyDiscrimiS2a', 'everyDiscrimiS3a', 'everyDiscrimiS4a', 'everyDiscrimiS5a')
# # Only include the specified columns and replaced 999 with NA
# df <- df[, column_names]
# df[df == 999] <- NA
# 
# # Define the imputation methods for each variable
# imp_methods <- make.method(df)
# # Predictive mean matching (pmm) method for continuous variables 
# imp_methods[column_names] <- "pmm"
# 
# # Imputation
# imp_data <- mice(df, m = 5, maxit = 50, method = imp_methods, seed = 500)
# completed_data <- complete(imp_data)
# write_xlsx(completed_data, path = "/Users/chia-chuanyu/Library/Mobile Documents/com~apple~CloudDocs/Chuan's Macbook Pro/My Ph.D/UT Austin/Research/Dissertation/publications/study1_onlineSurvey/final_imputed.xlsx")
# head(completed_data)
```

# Multivariate normality and outlier detection

Data violated multivariate normality assumption, thus in hypothesis
testing we used log transformation after removing outliers.

``` r
# Test multivariate normality
data <- read_excel("/Users/chia-chuanyu/Desktop/dissertationData/study1/study1_organizedFinal.xlsx", sheet = "finalForAnalyze")
# vars <- c('eai', 'edeqs', 'phq_9', 'mossss_overallSupportIndex', 'srq', 'fomos', 'everyDiscrimiS')
vars <- c('sex', 'eai', 'edeqs', 'phq_9', 'ipaqsf_totalMET', 'mossss_overallSupportIndex', 'srq', 'fomos', 'everyDiscrimiS')
data <- data[vars]
data_mn1 <- mvn(data, mvn_test = "mardia", scale = TRUE, descriptives = TRUE, bootstrap = TRUE, B = 10000)
summary(data_mn1)
```

    ## 

    ## ── Multivariate Normality Test Results ─────────────────────────────────────────

    ##              Test Statistic p.value    Method N.Boot          MVN
    ## 1 Mardia Skewness  1264.949  <0.001 bootstrap  10000 ✗ Not normal
    ## 2 Mardia Kurtosis    15.893  <0.001 bootstrap  10000 ✗ Not normal

    ## 

    ## ── Univariate Normality Test Results ───────────────────────────────────────────

    ##               Test                   Variable Statistic p.value    Normality
    ## 1 Anderson-Darling                        sex    81.551  <0.001 ✗ Not normal
    ## 2 Anderson-Darling                        eai     0.572   0.137     ✓ Normal
    ## 3 Anderson-Darling                      edeqs     3.783  <0.001 ✗ Not normal
    ## 4 Anderson-Darling                      phq_9     2.764  <0.001 ✗ Not normal
    ## 5 Anderson-Darling            ipaqsf_totalMET     9.525  <0.001 ✗ Not normal
    ## 6 Anderson-Darling mossss_overallSupportIndex     0.967   0.015 ✗ Not normal
    ## 7 Anderson-Darling                        srq     0.278   0.648     ✓ Normal
    ## 8 Anderson-Darling                      fomos     1.467  <0.001 ✗ Not normal
    ## 9 Anderson-Darling             everyDiscrimiS     6.785  <0.001 ✗ Not normal

    ## 

    ## ── Descriptive Statistics ──────────────────────────────────────────────────────

    ##                     Variable   n     Mean  Std.Dev   Median   Min      Max
    ## 1                        sex 225   46.013  205.985    2.000 1.000   999.00
    ## 2                        eai 225   20.138    6.250   20.000 6.000    33.00
    ## 3                      edeqs 225   10.031    7.721    9.000 0.000    32.00
    ## 4                      phq_9 225    8.698    6.236    8.000 0.000    27.00
    ## 5            ipaqsf_totalMET 225 4539.794 3793.910 3732.000 0.000 26991.00
    ## 6 mossss_overallSupportIndex 225    3.656    0.849    3.632 1.158     5.00
    ## 7                        srq 225    4.530    0.541    4.522 2.826     5.87
    ## 8                      fomos 225    2.475    0.762    2.400 1.000     4.60
    ## 9             everyDiscrimiS 225    6.347    4.730    6.000 0.000    22.00
    ##       25th     75th   Skew Kurtosis
    ## 1    1.000    2.000  4.421   20.546
    ## 2   16.000   25.000 -0.086    2.465
    ## 3    3.000   16.000  0.508    2.284
    ## 4    3.000   13.000  0.531    2.506
    ## 5 2253.000 5826.000  2.294   10.873
    ## 6    3.158    4.263 -0.393    2.877
    ## 7    4.130    4.870 -0.045    3.084
    ## 8    1.900    3.000  0.314    2.338
    ## 9    3.000    8.000  1.325    4.866

``` r
# Outliers detection with Mahalanobis distance before log transformation
distances <- mahalanobis(data, colMeans(data), cov(data))
threshold <- qchisq(0.99, df = ncol(data))  # 99% CI (i.e., only remove extreme individuals)
outliers <- which(distances > threshold)
print(outliers)
```

    ##  [1]  12  84  85  91 123 126 128 166 194 207 211 212 215 222 223

``` r
print(length(outliers))
```

    ## [1] 15

``` r
data_no_outliers <- data[-outliers, ]

# Test multivariate normality again
data_mn2 <- mvn(data_no_outliers, mvn_test = "mardia", scale = TRUE, descriptives = TRUE, bootstrap = TRUE, B = 10000)
summary(data_mn2)
```

    ## 

    ## ── Multivariate Normality Test Results ─────────────────────────────────────────

    ##              Test Statistic p.value    Method N.Boot          MVN
    ## 1 Mardia Skewness   431.278  <0.001 bootstrap  10000 ✗ Not normal
    ## 2 Mardia Kurtosis     5.481  <0.001 bootstrap  10000 ✗ Not normal

    ## 

    ## ── Univariate Normality Test Results ───────────────────────────────────────────

    ##               Test                   Variable Statistic p.value    Normality
    ## 1 Anderson-Darling                        sex    31.599  <0.001 ✗ Not normal
    ## 2 Anderson-Darling                        eai     0.539   0.165     ✓ Normal
    ## 3 Anderson-Darling                      edeqs     3.503  <0.001 ✗ Not normal
    ## 4 Anderson-Darling                      phq_9     3.014  <0.001 ✗ Not normal
    ## 5 Anderson-Darling            ipaqsf_totalMET     5.712  <0.001 ✗ Not normal
    ## 6 Anderson-Darling mossss_overallSupportIndex     0.928   0.018 ✗ Not normal
    ## 7 Anderson-Darling                        srq     0.310   0.552     ✓ Normal
    ## 8 Anderson-Darling                      fomos     1.508  <0.001 ✗ Not normal
    ## 9 Anderson-Darling             everyDiscrimiS     6.707  <0.001 ✗ Not normal

    ## 

    ## ── Descriptive Statistics ──────────────────────────────────────────────────────

    ##                     Variable   n     Mean  Std.Dev   Median   Min      Max
    ## 1                        sex 210    1.686    0.576    2.000 1.000     5.00
    ## 2                        eai 210   20.219    6.152   20.000 6.000    33.00
    ## 3                      edeqs 210    9.824    7.555    9.000 0.000    28.00
    ## 4                      phq_9 210    8.490    6.220    8.000 0.000    24.00
    ## 5            ipaqsf_totalMET 210 4232.189 3100.502 3625.200 0.000 20490.60
    ## 6 mossss_overallSupportIndex 210    3.678    0.828    3.658 1.158     5.00
    ## 7                        srq 210    4.543    0.531    4.522 3.000     5.87
    ## 8                      fomos 210    2.440    0.754    2.400 1.000     4.60
    ## 9             everyDiscrimiS 210    6.229    4.765    5.500 0.000    22.00
    ##       25th     75th   Skew Kurtosis
    ## 1    1.000    2.000  1.051    8.558
    ## 2   16.000   24.000 -0.063    2.512
    ## 3    3.000   15.750  0.462    2.181
    ## 4    3.000   13.000  0.522    2.367
    ## 5 2258.250 5504.500  1.650    7.173
    ## 6    3.158    4.303 -0.339    2.816
    ## 7    4.174    4.913  0.050    2.919
    ## 8    1.800    3.000  0.345    2.377
    ## 9    3.000    8.000  1.360    4.940

``` r
# Print variables with 0 values, which are added by 1 in log transformation
colSums(data_no_outliers[, vars] == 0, na.rm = TRUE) > 0
```

    ##                        sex                        eai 
    ##                      FALSE                      FALSE 
    ##                      edeqs                      phq_9 
    ##                       TRUE                       TRUE 
    ##            ipaqsf_totalMET mossss_overallSupportIndex 
    ##                       TRUE                      FALSE 
    ##                        srq                      fomos 
    ##                      FALSE                      FALSE 
    ##             everyDiscrimiS 
    ##                       TRUE

``` r
# Check multivariate normality after log transformation, just check.
data_clean <- data_no_outliers %>%
  mutate(
    eai  = log(eai),
    edeqs = log(edeqs + 1),
    phq_9 = log(phq_9 + 1),
    ipaqsf_totalMET = log(ipaqsf_totalMET + 1),
    mossss_overallSupportIndex = log(mossss_overallSupportIndex),
    srq  = log(srq),
    fomos = log(fomos),
    everyDiscrimiS = log(everyDiscrimiS + 1)
  )

data_clean$eai <- scale(data_clean$eai)
data_clean$edeqs <- scale(data_clean$edeqs)
data_clean$phq_9 <- scale(data_clean$phq_9)
data_clean$ipaqsf_totalMET <- scale(data_clean$ipaqsf_totalMET)
data_clean$mossss_overallSupportIndex <- scale(data_clean$mossss_overallSupportIndex)
data_clean$srq <- scale(data_clean$srq)
data_clean$fomos <- scale(data_clean$fomos)
data_clean$everyDiscrimiS <- scale(data_clean$everyDiscrimiS)

data_mn3 <- mvn(data_clean, mvn_test = "mardia", scale = TRUE, descriptives = TRUE, bootstrap = TRUE, B = 10000)
summary(data_mn3)
```

    ## 

    ## ── Multivariate Normality Test Results ─────────────────────────────────────────

    ##              Test Statistic p.value    Method N.Boot          MVN
    ## 1 Mardia Skewness   997.514  <0.001 bootstrap  10000 ✗ Not normal
    ## 2 Mardia Kurtosis    19.943  <0.001 bootstrap  10000 ✗ Not normal

    ## 

    ## ── Univariate Normality Test Results ───────────────────────────────────────────

    ##               Test                   Variable Statistic p.value    Normality
    ## 1 Anderson-Darling                        sex    31.599  <0.001 ✗ Not normal
    ## 2 Anderson-Darling                        eai     2.819  <0.001 ✗ Not normal
    ## 3 Anderson-Darling                      edeqs     6.275  <0.001 ✗ Not normal
    ## 4 Anderson-Darling                      phq_9     4.930  <0.001 ✗ Not normal
    ## 5 Anderson-Darling            ipaqsf_totalMET     8.388  <0.001 ✗ Not normal
    ## 6 Anderson-Darling mossss_overallSupportIndex     2.960  <0.001 ✗ Not normal
    ## 7 Anderson-Darling                        srq     0.365   0.434     ✓ Normal
    ## 8 Anderson-Darling                      fomos     0.980   0.014 ✗ Not normal
    ## 9 Anderson-Darling             everyDiscrimiS     3.747  <0.001 ✗ Not normal

    ## 

    ## ── Descriptive Statistics ──────────────────────────────────────────────────────

    ##                     Variable   n  Mean Std.Dev Median    Min   Max   25th  75th
    ## 1                        sex 210 1.686   0.576  2.000  1.000 5.000  1.000 2.000
    ## 2                        eai 210 0.000   1.000  0.124 -3.323 1.558 -0.515 0.646
    ## 3                      edeqs 210 0.000   1.000  0.288 -2.058 1.372 -0.646 0.813
    ## 4                      phq_9 210 0.000   1.000  0.278 -2.253 1.455 -0.656 0.787
    ## 5            ipaqsf_totalMET 210 0.000   1.000  0.163 -7.135 1.705 -0.258 0.535
    ## 6 mossss_overallSupportIndex 210 0.000   1.000  0.093 -4.439 1.324 -0.486 0.732
    ## 7                        srq 210 0.000   1.000  0.018 -3.441 2.218 -0.657 0.718
    ## 8                      fomos 210 0.000   1.000  0.103 -2.630 2.134 -0.795 0.800
    ## 9             everyDiscrimiS 210 0.000   1.000  0.162 -2.405 1.902 -0.500 0.614
    ##     Skew Kurtosis
    ## 1  1.051    8.558
    ## 2 -1.022    4.301
    ## 3 -0.782    2.578
    ## 4 -0.788    2.794
    ## 5 -3.661   25.872
    ## 6 -1.188    5.244
    ## 7 -0.309    3.239
    ## 8 -0.261    2.426
    ## 9 -0.677    3.466

# Sample characteristics

Note that the values here are raw data after outlier removal.

``` r
# Load the data again, as some variables were not included in normality and outlier detection steps.
data_des <- read_excel("/Users/chia-chuanyu/Desktop/dissertationData/study1/study1_organizedFinal.xlsx", sheet = "finalForAnalyze")
data_des <- data_des[-outliers, ]
catVars <- c("sex", "race", 'gender', 'ethnicity', 'selfDegree', 'marry', 'selfIncome', 'employment', 'past6monthAthleticCompetition', 'everAthleticCompetition')
CreateTableOne(data = data_des, vars = catVars, factorVars = catVars)
```

    ##                                        
    ##                                         Overall    
    ##   n                                     210        
    ##   sex (%)                                          
    ##      1                                   73 (34.8) 
    ##      2                                  134 (63.8) 
    ##      4                                    2 ( 1.0) 
    ##      5                                    1 ( 0.5) 
    ##   race (%)                                         
    ##      1                                  129 (61.4) 
    ##      2                                   10 ( 4.8) 
    ##      3                                    1 ( 0.5) 
    ##      4                                   65 (31.0) 
    ##      999                                  5 ( 2.4) 
    ##   gender (%)                                       
    ##      1                                   70 (33.3) 
    ##      2                                  129 (61.4) 
    ##      3                                    2 ( 1.0) 
    ##      4                                    1 ( 0.5) 
    ##      5                                    2 ( 1.0) 
    ##      6                                    6 ( 2.9) 
    ##   ethnicity (%)                                    
    ##      1                                   59 (28.1) 
    ##      2                                   74 (35.2) 
    ##      3                                    8 ( 3.8) 
    ##      5                                   62 (29.5) 
    ##      7                                    2 ( 1.0) 
    ##      8                                    4 ( 1.9) 
    ##      999                                  1 ( 0.5) 
    ##   selfDegree (%)                                   
    ##      1                                    1 ( 0.5) 
    ##      2                                   45 (21.4) 
    ##      3                                   61 (29.0) 
    ##      4                                   52 (24.8) 
    ##      5                                   47 (22.4) 
    ##      6                                    4 ( 1.9) 
    ##   marry (%)                                        
    ##      1                                  174 (82.9) 
    ##      2                                   33 (15.7) 
    ##      5                                    2 ( 1.0) 
    ##      999                                  1 ( 0.5) 
    ##   selfIncome (%)                                   
    ##      0                                   43 (20.5) 
    ##      1                                   59 (28.1) 
    ##      2                                   27 (12.9) 
    ##      3                                   44 (21.0) 
    ##      4                                   19 ( 9.0) 
    ##      5                                   13 ( 6.2) 
    ##      6                                    2 ( 1.0) 
    ##      7                                    1 ( 0.5) 
    ##      999                                  2 ( 1.0) 
    ##   employment (%)                                   
    ##      0                                    7 ( 3.3) 
    ##      1                                  109 (51.9) 
    ##      2                                   50 (23.8) 
    ##      3                                   37 (17.6) 
    ##      4                                    2 ( 1.0) 
    ##      5                                    4 ( 1.9) 
    ##      999                                  1 ( 0.5) 
    ##   past6monthAthleticCompetition = 1 (%)  60 (28.6) 
    ##   everAthleticCompetition (%)                      
    ##      0                                   68 (32.4) 
    ##      1                                  141 (67.1) 
    ##      999                                  1 ( 0.5)

``` r
conVars <- c('age', 'ipaqsf_totalMET', 'eai', 'edeqs', 'phq_9', 'mossss_overallSupportIndex', 'srq', 'fomos', 'everyDiscrimiS')
describe(data_des[, conVars])
```

    ##                            vars   n    mean      sd  median trimmed     mad
    ## age                           1 210   23.82    4.71   22.00   23.40    4.45
    ## ipaqsf_totalMET               2 210 4232.19 3100.50 3625.20 3824.25 2324.72
    ## eai                           3 210   20.22    6.15   20.00   20.26    5.93
    ## edeqs                         4 210    9.82    7.56    9.00    9.33    8.90
    ## phq_9                         5 210    8.49    6.22    8.00    8.04    7.41
    ## mossss_overallSupportIndex    6 210    3.68    0.83    3.66    3.71    0.82
    ## srq                           7 210    4.54    0.53    4.52    4.54    0.58
    ## fomos                         8 210    2.44    0.75    2.40    2.41    0.89
    ## everyDiscrimiS                9 210    6.23    4.76    5.50    5.57    3.71
    ##                              min      max    range  skew kurtosis     se
    ## age                        18.00    35.00    17.00  0.64    -0.67   0.32
    ## ipaqsf_totalMET             0.00 20490.60 20490.60  1.64     4.11 213.95
    ## eai                         6.00    33.00    27.00 -0.06    -0.51   0.42
    ## edeqs                       0.00    28.00    28.00  0.46    -0.84   0.52
    ## phq_9                       0.00    24.00    24.00  0.52    -0.66   0.43
    ## mossss_overallSupportIndex  1.16     5.00     3.84 -0.34    -0.21   0.06
    ## srq                         3.00     5.87     2.87  0.05    -0.11   0.04
    ## fomos                       1.00     4.60     3.60  0.34    -0.65   0.05
    ## everyDiscrimiS              0.00    22.00    22.00  1.35     1.89   0.33

``` r
print("The # and % of people with EAI score ≥ 29 (the cutoff for high-risk by Szabo, A., Pinto, A., Griffiths, M. D., Kovacsik, R., & Demetrovics, Z. (2019). The psychometric evaluation of the Revised Exercise Addiction Inventory: Improved psychometric properties by changing item response rating. Journal of Behavioral Addictions, 8(1), 157-161.)")
```

    ## [1] "The # and % of people with EAI score ≥ 29 (the cutoff for high-risk by Szabo, A., Pinto, A., Griffiths, M. D., Kovacsik, R., & Demetrovics, Z. (2019). The psychometric evaluation of the Revised Exercise Addiction Inventory: Improved psychometric properties by changing item response rating. Journal of Behavioral Addictions, 8(1), 157-161.)"

``` r
print(sum(data_des$eai >= 29, na.rm = TRUE))
```

    ## [1] 24

``` r
print((sum(data_des$eai >= 29, na.rm = TRUE)) / sum(!is.na(data_des$eai)))
```

    ## [1] 0.1142857

# Simple Correlation for EA

No p-value adjestment becasue we are exploring the data.  
Spearman is used becasue the data violated multivariate normality.

``` r
# Specify vars again, since we don't use sex in correlation, which was used in outlier detection.
vars <- c('eai', 'edeqs', 'phq_9', 'mossss_overallSupportIndex', 'srq', 'fomos', 'everyDiscrimiS', 'ipaqsf_totalMET')
corr <- corr.test(
  data_clean[, c(vars)],
  use = "pairwise", method = "spearman", adjust = "none")
new_labels <- c("EA", "ED", "Depress", "SS", "SR", "FoMO", "Discrim", "PA")
rownames(corr$r) <- new_labels
colnames(corr$r) <- new_labels
rownames(corr$p) <- new_labels
colnames(corr$p) <- new_labels
corr$stars
```

    ##                            eai       edeqs     phq_9     
    ## eai                        "1***"    "0.26***" "0.13 "   
    ## edeqs                      "0.26***" "1***"    "0.6***"  
    ## phq_9                      "0.13 "   "0.6***"  "1***"    
    ## mossss_overallSupportIndex "-0.02 "  "-0.14*"  "-0.31***"
    ## srq                        "0.19**"  "0.24***" "0.29***" 
    ## fomos                      "0.16*"   "0.45***" "0.53***" 
    ## everyDiscrimiS             "0.16*"   "0.3***"  "0.39***" 
    ## ipaqsf_totalMET            "0.32***" "0.11 "   "0.06 "   
    ##                            mossss_overallSupportIndex srq       fomos    
    ## eai                        "-0.02 "                   "0.19**"  "0.16*"  
    ## edeqs                      "-0.14*"                   "0.24***" "0.45***"
    ## phq_9                      "-0.31***"                 "0.29***" "0.53***"
    ## mossss_overallSupportIndex "1***"                     "-0.02 "  "-0.12 " 
    ## srq                        "-0.02 "                   "1***"    "0.36***"
    ## fomos                      "-0.12 "                   "0.36***" "1***"   
    ## everyDiscrimiS             "-0.14*"                   "0.32***" "0.36***"
    ## ipaqsf_totalMET            "0.03 "                    "0.16*"   "0.06 "  
    ##                            everyDiscrimiS ipaqsf_totalMET
    ## eai                        "0.16*"        "0.32***"      
    ## edeqs                      "0.3***"       "0.11 "        
    ## phq_9                      "0.39***"      "0.06 "        
    ## mossss_overallSupportIndex "-0.14*"       "0.03 "        
    ## srq                        "0.32***"      "0.16*"        
    ## fomos                      "0.36***"      "0.06 "        
    ## everyDiscrimiS             "1***"         "0.03 "        
    ## ipaqsf_totalMET            "0.03 "        "1***"

``` r
p <- ggcorrplot.mixed(corr$r, upper = "ellipse", p.mat = corr$p, insig = "label_sig", sig.lvl = c(.05, .01, .001))
col1 <- colorRampPalette(c("#00007F", "blue", "#007FFF", "#FF7F00", "red", "#7F0000"))
p <- p + scale_fill_gradientn(colours = col1(10), limits = c(-1, 1),
  guide = guide_colorbar(direction = "horizontal", title = "", nbin = 1000,
  ticks.colour = "black", frame.colour = "black", barwidth = 15, barheight = 1.5)) +
  scale_colour_gradientn(colours = col1(10), limits = c(-1, 1),
  guide = guide_colorbar(direction = "horizontal", title = "", nbin = 1000,
  ticks.colour = "black", frame.colour = "black", barwidth = 15, barheight = 1.5)) +
  #ggtitle("Spearman Correlation between Variables") +
  theme(legend.position = "bottom", plot.title = element_text(hjust = 0.5, size = 14, face = "bold"))
```

    ## Scale for fill is already present.
    ## Adding another scale for fill, which will replace the existing scale.
    ## Scale for colour is already present.
    ## Adding another scale for colour, which will replace the existing scale.

``` r
p
```

![](/Users/chia-chuanyu/Chuan's%20Macbook%20Pro/My%20Ph.D/UT%20Austin/Research/Dissertation/publications/study1_onlineSurvey/Relationship-between-Exercise-Addiction-Risk-and-Social-Interaction-is-Moderated-by-Depression-and-Eating-Disorder_files/figure-gfm/corr-plot-1.png)<!-- -->

``` r
ggsave(filename = "correlation_plot.png", plot = p, width = 7, height = 7, dpi = 300)

# Our non-parametric correlation showed that EA is positively correlated with eating disorder, depression, social reward, fear of missing out, perceived discrimination, and physical activity.
```

# Partial Correlation for PA

``` r
paVars <- c("eai", "mossss_overallSupportIndex", "srq", "fomos", "everyDiscrimiS")
covariates <- c("sex", "edeqs", "phq_9")

results <- data.frame("Social-PA Correlations when Controlling for Covariates" = paVars, Partial_r = NA, p_value = NA)

for (i in seq_along(paVars)) {
  df_subset <- data_clean[, c("ipaqsf_totalMET", paVars[i], covariates)]
  pcor_res <- pcor(df_subset, method = "spearman")
  results$Partial_r[i] <- pcor_res$estimate[1, 2]
  results$p_value[i] <- pcor_res$p.value[1, 2]
}
results
```

    ##   Social.PA.Correlations.when.Controlling.for.Covariates   Partial_r
    ## 1                                                    eai 0.293718363
    ## 2                             mossss_overallSupportIndex 0.067894851
    ## 3                                                    srq 0.139262071
    ## 4                                                  fomos 0.005688143
    ## 5                                         everyDiscrimiS 0.014797965
    ##        p_value
    ## 1 0.0000174373
    ## 2 0.3310287876
    ## 3 0.0453641834
    ## 4 0.9351690052
    ## 5 0.8323972756

# Log transformation and scaling

``` r
# data_clean <- data_clean %>%
#   mutate(
#     eai  = log(eai),
#     edeqs = log(edeqs + 1),
#     phq_9 = log(phq_9 + 1),
#     ipaqsf_totalMET = log(ipaqsf_totalMET + 1),
#     mossss_overallSupportIndex = log(mossss_overallSupportIndex),
#     srq  = log(srq),
#     fomos = log(fomos),
#     everyDiscrimiS = log(everyDiscrimiS + 1)
#   )
# data_clean$eai <- scale(data_clean$eai)
# data_clean$edeqs <- scale(data_clean$edeqs)
# data_clean$phq_9 <- scale(data_clean$phq_9)
# data_clean$ipaqsf_totalMET <- scale(data_clean$ipaqsf_totalMET)
# data_clean$mossss_overallSupportIndex <- scale(data_clean$mossss_overallSupportIndex)
# data_clean$srq <- scale(data_clean$srq)
# data_clean$fomos <- scale(data_clean$fomos)
# data_clean$everyDiscrimiS <- scale(data_clean$everyDiscrimiS)
```

# Hypothesis testing for social support (SS)

``` r
model.ss <- lm(eai ~ mossss_overallSupportIndex + sex + ipaqsf_totalMET + phq_9 + edeqs +
                 mossss_overallSupportIndex*edeqs, data = data_clean)
summary(model.ss)
```

    ## 
    ## Call:
    ## lm(formula = eai ~ mossss_overallSupportIndex + sex + ipaqsf_totalMET + 
    ##     phq_9 + edeqs + mossss_overallSupportIndex * edeqs, data = data_clean)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.4299 -0.4375  0.0735  0.6437  2.7875 
    ## 
    ## Coefficients:
    ##                                  Estimate Std. Error t value Pr(>|t|)  
    ## (Intercept)                       0.29550    0.21705   1.361   0.1749  
    ## mossss_overallSupportIndex       -0.01088    0.07260  -0.150   0.8811  
    ## sex                              -0.17085    0.12313  -1.388   0.1668  
    ## ipaqsf_totalMET                   0.17840    0.07067   2.524   0.0124 *
    ## phq_9                            -0.04927    0.08823  -0.558   0.5771  
    ## edeqs                             0.17885    0.08766   2.040   0.0426 *
    ## mossss_overallSupportIndex:edeqs  0.05237    0.08334   0.628   0.5304  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9735 on 203 degrees of freedom
    ## Multiple R-squared:  0.07944,    Adjusted R-squared:  0.05223 
    ## F-statistic: 2.919 on 6 and 203 DF,  p-value: 0.00941

``` r
print(lm.beta::lm.beta(model.ss))
```

    ## 
    ## Call:
    ## lm(formula = eai ~ mossss_overallSupportIndex + sex + ipaqsf_totalMET + 
    ##     phq_9 + edeqs + mossss_overallSupportIndex * edeqs, data = data_clean)
    ## 
    ## Standardized Coefficients::
    ##                      (Intercept)       mossss_overallSupportIndex 
    ##                               NA                      -0.01087702 
    ##                              sex                  ipaqsf_totalMET 
    ##                      -0.09834721                       0.17839878 
    ##                            phq_9                            edeqs 
    ##                      -0.04927163                       0.17884547 
    ## mossss_overallSupportIndex:edeqs 
    ##                       0.04397163

``` r
print(confint(model.ss))
```

    ##                                         2.5 %     97.5 %
    ## (Intercept)                      -0.132471945 0.72346462
    ## mossss_overallSupportIndex       -0.154022955 0.13226892
    ## sex                              -0.413623580 0.07193309
    ## ipaqsf_totalMET                   0.039061985 0.31773557
    ## phq_9                            -0.223227649 0.12468438
    ## edeqs                             0.005999242 0.35169170
    ## mossss_overallSupportIndex:edeqs -0.111951836 0.21669866

``` r
p1 <- interact_plot(model.ss, pred = "mossss_overallSupportIndex", modx = "edeqs",
                    x.label = "Social Support", y.label = "EA Risk", interval = TRUE,
                    colors = c("#0072B2", "#E69F00", "#009E73")) +
                    theme_classic(base_size = 12) + theme(legend.position = "none")
p1
```

![](/Users/chia-chuanyu/Chuan's%20Macbook%20Pro/My%20Ph.D/UT%20Austin/Research/Dissertation/publications/study1_onlineSurvey/Relationship-between-Exercise-Addiction-Risk-and-Social-Interaction-is-Moderated-by-Depression-and-Eating-Disorder_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

# Hypothesis testing for social reward (SR)

``` r
model.sr <- lm(eai ~ srq + sex + ipaqsf_totalMET + phq_9 + edeqs + 
                   srq*edeqs, data = data_clean)
summary(model.sr)
```

    ## 
    ## Call:
    ## lm(formula = eai ~ srq + sex + ipaqsf_totalMET + phq_9 + edeqs + 
    ##     srq * edeqs, data = data_clean)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.5340 -0.4670  0.1063  0.6464  2.6311 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)  
    ## (Intercept)      0.25196    0.21402   1.177   0.2405  
    ## srq              0.06685    0.07176   0.932   0.3526  
    ## sex             -0.16708    0.11993  -1.393   0.1651  
    ## ipaqsf_totalMET  0.16242    0.06929   2.344   0.0200 *
    ## phq_9           -0.06448    0.08575  -0.752   0.4529  
    ## edeqs            0.19722    0.08642   2.282   0.0235 *
    ## srq:edeqs        0.12906    0.07362   1.753   0.0811 .
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9628 on 203 degrees of freedom
    ## Multiple R-squared:  0.09955,    Adjusted R-squared:  0.07293 
    ## F-statistic:  3.74 on 6 and 203 DF,  p-value: 0.001495

``` r
print(lm.beta::lm.beta(model.sr))
```

    ## 
    ## Call:
    ## lm(formula = eai ~ srq + sex + ipaqsf_totalMET + phq_9 + edeqs + 
    ##     srq * edeqs, data = data_clean)
    ## 
    ## Standardized Coefficients::
    ##     (Intercept)             srq             sex ipaqsf_totalMET           phq_9 
    ##              NA      0.06685224     -0.09618123      0.16241860     -0.06448281 
    ##           edeqs       srq:edeqs 
    ##      0.19722471      0.12110624

``` r
print(confint(model.sr))
```

    ##                       2.5 %     97.5 %
    ## (Intercept)     -0.17002674 0.67394642
    ## srq             -0.07463243 0.20833691
    ## sex             -0.40355821 0.06939303
    ## ipaqsf_totalMET  0.02580445 0.29903274
    ## phq_9           -0.23356020 0.10459458
    ## edeqs            0.02682844 0.36762098
    ## srq:edeqs       -0.01610691 0.27422240

``` r
sim_slopes(model.sr, pred = srq, modx = edeqs)
```

    ## JOHNSON-NEYMAN INTERVAL
    ## 
    ## When edeqs is INSIDE the interval [0.66, 5.37], the slope of srq is p <
    ## .05.
    ## 
    ## Note: The range of observed values of edeqs is [-2.06, 1.37]
    ## 
    ## SIMPLE SLOPES ANALYSIS
    ## 
    ## Slope of srq when edeqs = -1.000000e+00 (- 1 SD): 
    ## 
    ##    Est.   S.E.   t val.      p
    ## ------- ------ -------- ------
    ##   -0.06   0.11    -0.55   0.59
    ## 
    ## Slope of srq when edeqs =  1.194018e-15 (Mean): 
    ## 
    ##   Est.   S.E.   t val.      p
    ## ------ ------ -------- ------
    ##   0.07   0.07     0.93   0.35
    ## 
    ## Slope of srq when edeqs =  1.000000e+00 (+ 1 SD): 
    ## 
    ##   Est.   S.E.   t val.      p
    ## ------ ------ -------- ------
    ##   0.20   0.09     2.17   0.03

``` r
p2 <- interact_plot(model.sr, pred = "srq", modx = "edeqs",
                    x.label = "Social Reward", y.label = "EA Risk", interval = TRUE, legend.main = "Eating Disorder Symptoms",
                    colors = c("#0072B2", "#E69F00", "#009E73")) +
                    theme_classic(base_size = 12)
p2
```

![](/Users/chia-chuanyu/Chuan's%20Macbook%20Pro/My%20Ph.D/UT%20Austin/Research/Dissertation/publications/study1_onlineSurvey/Relationship-between-Exercise-Addiction-Risk-and-Social-Interaction-is-Moderated-by-Depression-and-Eating-Disorder_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

# Hypothesis testing for fear of missing out (fomos)

``` r
model.fomos <- lm(eai ~ fomos + sex + ipaqsf_totalMET + phq_9 + edeqs + 
                   fomos*edeqs, data = data_clean)
summary(model.fomos)
```

    ## 
    ## Call:
    ## lm(formula = eai ~ fomos + sex + ipaqsf_totalMET + phq_9 + edeqs + 
    ##     fomos * edeqs, data = data_clean)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.2788 -0.4667  0.1149  0.6559  2.7802 
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)      0.19291    0.21467   0.899  0.36992   
    ## fomos            0.08878    0.07773   1.142  0.25478   
    ## sex             -0.15851    0.11901  -1.332  0.18436   
    ## ipaqsf_totalMET  0.18692    0.06898   2.710  0.00731 **
    ## phq_9           -0.07483    0.08915  -0.839  0.40223   
    ## edeqs            0.19554    0.08649   2.261  0.02482 * 
    ## fomos:edeqs      0.17843    0.06841   2.608  0.00978 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9548 on 203 degrees of freedom
    ## Multiple R-squared:  0.1146, Adjusted R-squared:  0.08842 
    ## F-statistic: 4.379 on 6 and 203 DF,  p-value: 0.0003494

``` r
print(lm.beta::lm.beta(model.fomos))
```

    ## 
    ## Call:
    ## lm(formula = eai ~ fomos + sex + ipaqsf_totalMET + phq_9 + edeqs + 
    ##     fomos * edeqs, data = data_clean)
    ## 
    ## Standardized Coefficients::
    ##     (Intercept)           fomos             sex ipaqsf_totalMET           phq_9 
    ##              NA      0.08877581     -0.09124893      0.18691925     -0.07483232 
    ##           edeqs     fomos:edeqs 
    ##      0.19554456      0.17648873

``` r
print(confint(model.fomos))
```

    ##                       2.5 %     97.5 %
    ## (Intercept)     -0.23036047 0.61617171
    ## fomos           -0.06449316 0.24204479
    ## sex             -0.39315966 0.07613092
    ## ipaqsf_totalMET  0.05091700 0.32292150
    ## phq_9           -0.25060804 0.10094340
    ## edeqs            0.02501704 0.36607208
    ## fomos:edeqs      0.04354767 0.31331108

``` r
sim_slopes(model.fomos, pred = fomos, modx = edeqs)
```

    ## JOHNSON-NEYMAN INTERVAL
    ## 
    ## When edeqs is OUTSIDE the interval [-2.89, 0.40], the slope of fomos is p <
    ## .05.
    ## 
    ## Note: The range of observed values of edeqs is [-2.06, 1.37]
    ## 
    ## SIMPLE SLOPES ANALYSIS
    ## 
    ## Slope of fomos when edeqs = -1.000000e+00 (- 1 SD): 
    ## 
    ##    Est.   S.E.   t val.      p
    ## ------- ------ -------- ------
    ##   -0.09   0.11    -0.84   0.40
    ## 
    ## Slope of fomos when edeqs =  1.194018e-15 (Mean): 
    ## 
    ##   Est.   S.E.   t val.      p
    ## ------ ------ -------- ------
    ##   0.09   0.08     1.14   0.25
    ## 
    ## Slope of fomos when edeqs =  1.000000e+00 (+ 1 SD): 
    ## 
    ##   Est.   S.E.   t val.      p
    ## ------ ------ -------- ------
    ##   0.27   0.10     2.66   0.01

``` r
p3 <- interact_plot(model.fomos, pred = "fomos", modx = "edeqs",
                    x.label = "Fear of Missing Out", y.label = "EA Risk", interval = TRUE,
                    colors = c("#0072B2", "#E69F00", "#009E73")) +
                    theme_classic(base_size = 12) + theme(legend.position = "none")
p3
```

![](/Users/chia-chuanyu/Chuan's%20Macbook%20Pro/My%20Ph.D/UT%20Austin/Research/Dissertation/publications/study1_onlineSurvey/Relationship-between-Exercise-Addiction-Risk-and-Social-Interaction-is-Moderated-by-Depression-and-Eating-Disorder_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

# Hypothesis testing for discrimination (dis)

``` r
model.dis <- lm(eai ~ everyDiscrimiS + sex + ipaqsf_totalMET + phq_9 + edeqs + 
                   everyDiscrimiS*edeqs, data = data_clean)
summary(model.dis)
```

    ## 
    ## Call:
    ## lm(formula = eai ~ everyDiscrimiS + sex + ipaqsf_totalMET + phq_9 + 
    ##     edeqs + everyDiscrimiS * edeqs, data = data_clean)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -3.4907 -0.3976  0.1232  0.6208  2.6414 
    ## 
    ## Coefficients:
    ##                      Estimate Std. Error t value Pr(>|t|)  
    ## (Intercept)           0.29739    0.21342   1.394   0.1650  
    ## everyDiscrimiS        0.09010    0.07154   1.259   0.2093  
    ## sex                  -0.20355    0.11999  -1.696   0.0913 .
    ## ipaqsf_totalMET       0.17044    0.06885   2.475   0.0141 *
    ## phq_9                -0.10069    0.08670  -1.161   0.2469  
    ## edeqs                 0.22045    0.08750   2.519   0.0125 *
    ## everyDiscrimiS:edeqs  0.15282    0.06439   2.373   0.0186 *
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9575 on 203 degrees of freedom
    ## Multiple R-squared:  0.1095, Adjusted R-squared:  0.08321 
    ## F-statistic: 4.162 on 6 and 203 DF,  p-value: 0.0005735

``` r
print(lm.beta::lm.beta(model.dis))
```

    ## 
    ## Call:
    ## lm(formula = eai ~ everyDiscrimiS + sex + ipaqsf_totalMET + phq_9 + 
    ##     edeqs + everyDiscrimiS * edeqs, data = data_clean)
    ## 
    ## Standardized Coefficients::
    ##          (Intercept)       everyDiscrimiS                  sex 
    ##                   NA           0.09009595          -0.11717262 
    ##      ipaqsf_totalMET                phq_9                edeqs 
    ##           0.17044008          -0.10068842           0.22045120 
    ## everyDiscrimiS:edeqs 
    ##           0.16113856

``` r
print(confint(model.dis))
```

    ##                            2.5 %     97.5 %
    ## (Intercept)          -0.12340093 0.71819077
    ## everyDiscrimiS       -0.05096157 0.23115347
    ## sex                  -0.44012542 0.03302928
    ## ipaqsf_totalMET       0.03468516 0.30619499
    ## phq_9                -0.27164051 0.07026368
    ## edeqs                 0.04791751 0.39298489
    ## everyDiscrimiS:edeqs  0.02585422 0.27977580

``` r
sim_slopes(model.dis, pred = everyDiscrimiS, modx = edeqs)
```

    ## JOHNSON-NEYMAN INTERVAL
    ## 
    ## When edeqs is OUTSIDE the interval [-4.23, 0.38], the slope of
    ## everyDiscrimiS is p < .05.
    ## 
    ## Note: The range of observed values of edeqs is [-2.06, 1.37]
    ## 
    ## SIMPLE SLOPES ANALYSIS
    ## 
    ## Slope of everyDiscrimiS when edeqs = -1.000000e+00 (- 1 SD): 
    ## 
    ##    Est.   S.E.   t val.      p
    ## ------- ------ -------- ------
    ##   -0.06   0.10    -0.65   0.52
    ## 
    ## Slope of everyDiscrimiS when edeqs =  1.194018e-15 (Mean): 
    ## 
    ##   Est.   S.E.   t val.      p
    ## ------ ------ -------- ------
    ##   0.09   0.07     1.26   0.21
    ## 
    ## Slope of everyDiscrimiS when edeqs =  1.000000e+00 (+ 1 SD): 
    ## 
    ##   Est.   S.E.   t val.      p
    ## ------ ------ -------- ------
    ##   0.24   0.10     2.53   0.01

``` r
p4 <- interact_plot(model.dis, pred = "everyDiscrimiS", modx = "edeqs",
                    x.label = "Perceived Discrimination", y.label = "EA Risk", interval = TRUE,
                    colors = c("#0072B2", "#E69F00", "#009E73")) +
                    theme_classic(base_size = 12) + theme(legend.position = "none")
p4
```

![](/Users/chia-chuanyu/Chuan's%20Macbook%20Pro/My%20Ph.D/UT%20Austin/Research/Dissertation/publications/study1_onlineSurvey/Relationship-between-Exercise-Addiction-Risk-and-Social-Interaction-is-Moderated-by-Depression-and-Eating-Disorder_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

# Combine all regression plots

``` r
combined_plot <- (p1 | p2) / (p3 | p4) +plot_annotation(tag_levels = "a", tag_prefix = "(", tag_suffix = ")")
combined_plot
```

![](/Users/chia-chuanyu/Chuan's%20Macbook%20Pro/My%20Ph.D/UT%20Austin/Research/Dissertation/publications/study1_onlineSurvey/Relationship-between-Exercise-Addiction-Risk-and-Social-Interaction-is-Moderated-by-Depression-and-Eating-Disorder_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
ggsave("interaction_plots.png", plot = combined_plot, width = 10,height = 7, units = "in", dpi = 300)
```

# FDR Correction

Since We have fours models, we need to apply FDR to control for inflated
Type I error.

``` r
p_values <- c(#summary(model.ss)$coefficients["mossss_overallSupportIndex", "Pr(>|t|)"],
              summary(model.ss)$coefficients["mossss_overallSupportIndex:edeqs", "Pr(>|t|)"],
              #summary(model.sr)$coefficients["srq", "Pr(>|t|)"],
              summary(model.sr)$coefficients["srq:edeqs", "Pr(>|t|)"],
              #summary(model.fomos)$coefficients["fomos", "Pr(>|t|)"],
              summary(model.fomos)$coefficients["fomos:edeqs", "Pr(>|t|)"],
              #summary(model.dis)$coefficients["everyDiscrimiS", "Pr(>|t|)"],
              summary(model.dis)$coefficients["everyDiscrimiS:edeqs", "Pr(>|t|)"])
adjusted_p_values <- p.adjust(p_values, method = "fdr")
# Below are the fdr-corrected p values for the interactions of SS*ED, SR*ED, FoMOS*ED, and Dis*ED
adjusted_p_values
```

    ## [1] 0.53043376 0.10816141 0.03713267 0.03713267

``` r
print("mossss_overallSupportIndex:edeqs")
```

    ## [1] "mossss_overallSupportIndex:edeqs"

``` r
confint(emmeans(model.ss, ~ mossss_overallSupportIndex*edeqs), adjust = "fdr")
```

    ##  mossss_overallSupportIndex    edeqs emmean     SE  df lower.CL upper.CL
    ##                    -3.4e-16 1.33e-15 0.0075 0.0682 203   -0.127    0.142
    ## 
    ## Confidence level used: 0.95

``` r
print("srq:edeqs")
```

    ## [1] "srq:edeqs"

``` r
confint(emmeans(model.sr, ~ srq*edeqs), adjust = "fdr")
```

    ##        srq    edeqs  emmean     SE  df lower.CL upper.CL
    ##  -4.73e-15 1.33e-15 -0.0297 0.0686 203   -0.165    0.106
    ## 
    ## Confidence level used: 0.95

``` r
print("fomos:edeqs")
```

    ## [1] "fomos:edeqs"

``` r
confint(emmeans(model.fomos, ~ fomos*edeqs), adjust = "fdr")
```

    ##      fomos    edeqs  emmean     SE  df lower.CL upper.CL
    ##  -1.96e-15 1.33e-15 -0.0743 0.0718 203   -0.216   0.0672
    ## 
    ## Confidence level used: 0.95

``` r
print("everyDiscrimiS:edeqs")
```

    ## [1] "everyDiscrimiS:edeqs"

``` r
confint(emmeans(model.dis, ~ everyDiscrimiS*edeqs), adjust = "fdr")
```

    ##  everyDiscrimiS    edeqs  emmean     SE  df lower.CL upper.CL
    ##        1.25e-15 1.33e-15 -0.0457 0.0688 203   -0.181     0.09
    ## 
    ## Confidence level used: 0.95

# Supplementary Analysis Examining the Relationships between PA and Social Interaction Dimensions

``` r
print("PA-Social Support Relationship")
```

    ## [1] "PA-Social Support Relationship"

``` r
model.pa.ss <- lm(ipaqsf_totalMET ~ mossss_overallSupportIndex + sex + eai + phq_9 + edeqs +
                    mossss_overallSupportIndex*edeqs, data = data_clean)
summary(model.pa.ss)
```

    ## 
    ## Call:
    ## lm(formula = ipaqsf_totalMET ~ mossss_overallSupportIndex + sex + 
    ##     eai + phq_9 + edeqs + mossss_overallSupportIndex * edeqs, 
    ##     data = data_clean)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -6.4082 -0.3023  0.1329  0.5241  1.7282 
    ## 
    ## Coefficients:
    ##                                  Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)                       0.62957    0.20861   3.018  0.00287 **
    ## mossss_overallSupportIndex        0.09517    0.07069   1.346  0.17969   
    ## sex                              -0.38695    0.11790  -3.282  0.00121 **
    ## eai                               0.17062    0.06759   2.524  0.01235 * 
    ## phq_9                            -0.09754    0.08608  -1.133  0.25846   
    ## edeqs                             0.19815    0.08548   2.318  0.02144 * 
    ## mossss_overallSupportIndex:edeqs -0.15865    0.08082  -1.963  0.05101 . 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9521 on 203 degrees of freedom
    ## Multiple R-squared:  0.1196, Adjusted R-squared:  0.09355 
    ## F-statistic: 4.595 on 6 and 203 DF,  p-value: 0.000213

``` r
print(lm.beta::lm.beta(model.pa.ss))
```

    ## 
    ## Call:
    ## lm(formula = ipaqsf_totalMET ~ mossss_overallSupportIndex + sex + 
    ##     eai + phq_9 + edeqs + mossss_overallSupportIndex * edeqs, 
    ##     data = data_clean)
    ## 
    ## Standardized Coefficients::
    ##                      (Intercept)       mossss_overallSupportIndex 
    ##                               NA                       0.09517109 
    ##                              sex                              eai 
    ##                      -0.22274954                       0.17061987 
    ##                            phq_9                            edeqs 
    ##                      -0.09754191                       0.19815314 
    ## mossss_overallSupportIndex:edeqs 
    ##                      -0.13319795

``` r
print(confint(model.pa.ss))
```

    ##                                        2.5 %        97.5 %
    ## (Intercept)                       0.21825855  1.0408861349
    ## mossss_overallSupportIndex       -0.04420605  0.2345482177
    ## sex                              -0.61941393 -0.1544910924
    ## eai                               0.03735873  0.3038810184
    ## phq_9                            -0.26725773  0.0721739165
    ## edeqs                             0.02960937  0.3666969156
    ## mossss_overallSupportIndex:edeqs -0.31800208  0.0007051604

``` r
print("PA-Social Reward Relationship")
```

    ## [1] "PA-Social Reward Relationship"

``` r
model.pa.sr <- lm(ipaqsf_totalMET ~ srq + sex + eai + phq_9 + edeqs + srq*edeqs, data = data_clean)
summary(model.pa.sr)
```

    ## 
    ## Call:
    ## lm(formula = ipaqsf_totalMET ~ srq + sex + eai + phq_9 + edeqs + 
    ##     srq * edeqs, data = data_clean)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -6.6813 -0.2601  0.1332  0.4917  1.7276 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)  0.58191    0.21073   2.761  0.00628 **
    ## srq          0.04429    0.07181   0.617  0.53805   
    ## sex         -0.34692    0.11796  -2.941  0.00365 **
    ## eai          0.16227    0.06922   2.344  0.02004 * 
    ## phq_9       -0.13525    0.08530  -1.586  0.11440   
    ## edeqs        0.17663    0.08660   2.040  0.04268 * 
    ## srq:edeqs    0.01261    0.07414   0.170  0.86510   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9624 on 203 degrees of freedom
    ## Multiple R-squared:  0.1004, Adjusted R-squared:  0.07378 
    ## F-statistic: 3.775 on 6 and 203 DF,  p-value: 0.001383

``` r
print(lm.beta::lm.beta(model.pa.sr))
```

    ## 
    ## Call:
    ## lm(formula = ipaqsf_totalMET ~ srq + sex + eai + phq_9 + edeqs + 
    ##     srq * edeqs, data = data_clean)
    ## 
    ## Standardized Coefficients::
    ## (Intercept)         srq         sex         eai       phq_9       edeqs 
    ##          NA  0.04429345 -0.19970462  0.16227017 -0.13525297  0.17662890 
    ##   srq:edeqs 
    ##  0.01183398

``` r
print(confint(model.pa.sr))
```

    ##                    2.5 %      97.5 %
    ## (Intercept)  0.166406557  0.99740539
    ## srq         -0.097295943  0.18588285
    ## sex         -0.579511324 -0.11432810
    ## eai          0.025780869  0.29875946
    ## phq_9       -0.303450047  0.03294411
    ## edeqs        0.005880159  0.34737764
    ## srq:edeqs   -0.133570977  0.15879290

``` r
print("PA-FoMO Relationship")
```

    ## [1] "PA-FoMO Relationship"

``` r
model.pa.fomos <- lm(ipaqsf_totalMET ~ fomos + sex + eai + phq_9 + edeqs + fomos*edeqs, data = data_clean)
summary(model.pa.fomos)
```

    ## 
    ## Call:
    ## lm(formula = ipaqsf_totalMET ~ fomos + sex + eai + phq_9 + edeqs + 
    ##     fomos * edeqs, data = data_clean)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -6.6988 -0.3159  0.1035  0.4695  1.7665 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)  0.65018    0.21011   3.094  0.00225 **
    ## fomos        0.04016    0.07790   0.516  0.60675   
    ## sex         -0.35294    0.11688  -3.020  0.00286 **
    ## eai          0.18678    0.06892   2.710  0.00731 **
    ## phq_9       -0.14374    0.08870  -1.621  0.10667   
    ## edeqs        0.14647    0.08693   1.685  0.09355 . 
    ## fomos:edeqs -0.13260    0.06889  -1.925  0.05565 . 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9544 on 203 degrees of freedom
    ## Multiple R-squared:  0.1153, Adjusted R-squared:  0.08911 
    ## F-statistic: 4.408 on 6 and 203 DF,  p-value: 0.0003269

``` r
print(lm.beta::lm.beta(model.pa.fomos))
```

    ## 
    ## Call:
    ## lm(formula = ipaqsf_totalMET ~ fomos + sex + eai + phq_9 + edeqs + 
    ##     fomos * edeqs, data = data_clean)
    ## 
    ## Standardized Coefficients::
    ## (Intercept)       fomos         sex         eai       phq_9       edeqs 
    ##          NA  0.04016019 -0.20317199  0.18677668 -0.14373552  0.14646625 
    ## fomos:edeqs 
    ## -0.13116118

``` r
print(confint(model.pa.fomos))
```

    ##                   2.5 %       97.5 %
    ## (Intercept)  0.23589491  1.064469053
    ## fomos       -0.11344121  0.193761587
    ## sex         -0.58340269 -0.122483503
    ## eai          0.05087817  0.322675189
    ## phq_9       -0.31862131  0.031150269
    ## edeqs       -0.02493489  0.317867394
    ## fomos:edeqs -0.26844045  0.003233629

``` r
print("PA-Discrimination Relationship")
```

    ## [1] "PA-Discrimination Relationship"

``` r
model.pa.dis <- lm(ipaqsf_totalMET ~ everyDiscrimiS + sex + eai + phq_9 + edeqs + everyDiscrimiS*edeqs, data = data_clean)
summary(model.pa.dis)
```

    ## 
    ## Call:
    ## lm(formula = ipaqsf_totalMET ~ everyDiscrimiS + sex + eai + phq_9 + 
    ##     edeqs + everyDiscrimiS * edeqs, data = data_clean)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -6.5888 -0.2653  0.1143  0.4737  1.7031 
    ## 
    ## Coefficients:
    ##                      Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)           0.61368    0.21101   2.908  0.00404 **
    ## everyDiscrimiS        0.04255    0.07207   0.590  0.55561   
    ## sex                  -0.35632    0.11875  -3.000  0.00303 **
    ## eai                   0.17192    0.06945   2.475  0.01412 * 
    ## phq_9                -0.12728    0.08691  -1.464  0.14461   
    ## edeqs                 0.15933    0.08854   1.799  0.07344 . 
    ## everyDiscrimiS:edeqs -0.04354    0.06549  -0.665  0.50696   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9617 on 203 degrees of freedom
    ## Multiple R-squared:  0.1018, Adjusted R-squared:  0.07523 
    ## F-statistic: 3.834 on 6 and 203 DF,  p-value: 0.00121

``` r
print(lm.beta::lm.beta(model.pa.dis))
```

    ## 
    ## Call:
    ## lm(formula = ipaqsf_totalMET ~ everyDiscrimiS + sex + eai + phq_9 + 
    ##     edeqs + everyDiscrimiS * edeqs, data = data_clean)
    ## 
    ## Standardized Coefficients::
    ##          (Intercept)       everyDiscrimiS                  sex 
    ##                   NA           0.04254658          -0.20511504 
    ##                  eai                phq_9                edeqs 
    ##           0.17192471          -0.12727828           0.15932829 
    ## everyDiscrimiS:edeqs 
    ##          -0.04590696

``` r
print(confint(model.pa.dis))
```

    ##                            2.5 %      97.5 %
    ## (Intercept)           0.19761679  1.02974112
    ## everyDiscrimiS       -0.09955438  0.18464753
    ## sex                  -0.59046644 -0.12217055
    ## eai                   0.03498729  0.30886213
    ## phq_9                -0.29863985  0.04408328
    ## edeqs                -0.01525646  0.33391304
    ## everyDiscrimiS:edeqs -0.17266454  0.08559324

# Conclusion

- When covariates were considered, we found that EA was significantly
  correlated with greater FoMO and perceived discrimination only when
  eating disorder symptoms were high.
- These ED-social interactions remained significant after FDR
  correction.
- When covariates were considered, we found that PA was not correlated
  with any dimension of social interaction.
