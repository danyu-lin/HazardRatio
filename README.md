# **HazardRatio**

# **HazardRatio: A SAS macro to generate confidence intervals for the hazard ratio in randomized clinical trials**

HazardRatio is a SAS macro to generate Wald, Peto’s, and score confidence intervals (CIs) for the log hazard ratio in randomized clinical trials. The point estimate, standard error, and p-value for each method is also generated. The Wald CI is based on the maximum partial likelihood estimator (MPLE) and the corresponding Fisher information matrix. Peto’s CI is based on the log-rank statistic and its variance estimator (Peto et al., 1977). The score CI is constructed by inverting the partial-likelihood score test under the Cox proportional hazards model (Lin et al., 2016).

## **SYNOPSIS**

This macro is defined with the name “**HazardRatio**” as:

%macro **HazardRatio** (**Data** = Data, **Time** = Time, **Status** = Status, **Stratum** =, **Treatment** = Treatment, **Alpha** = 0.05, **Accuracy** = 0.001, **Result** = HR_CI);

……\
%mend **HazardRatio**;

## **OPTIONS**

| Option        | Default       | Description                                                                                                                                         |
|:--------------|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------|
| **Data**      | *`Data`*      | The dataset to be used                                                                                                                              |
| **Time**      | *`Time`*      | The event time                                                                                                                                      |
| **Status**    | *`Status`*    | Indicates, by the values 1 versus 0, whether the event time is observed or censored                                                                 |
| **Stratum**   | *`{NONE}`*    | Numeric value that defines the strata (for unstratified analysis, leave this argument blank)                                                        |
| **Treatment** | *`Treatment`* | A binary variable, e.g. 0 for placebo and 1 for test drug                                                                                           |
| **Alpha**     | *`0.05`*      | Confidence level (1-Alpha)                                                                                                                          |
| **Accuracy**  | *`0.001`*     | Accuracy of the score confidence limits                                                                                                             |
| **Result**    | *`HR_CI`*     | Output file storing the results, including point estimates, standard errors, CIs, test statistics, and p-values for Wald, Score, and Peto’s methods |

## **DATA**

### **Data file**

The file “colon” contains the data from a clinical trial on adjuvant therapy for patients with resected colon cancer. A snapshot of the data is provided below. The variable “Treatment” indicates, by the values 1 versus 0, whether the patient received the combined therapy of levamisole and fluorouracil or observation. The variable “Stratum” is based on the number of lymph nodes. The variable “Time” is the time point at which the patient is either censored (Status = 0) or observed to die (Status = 1).

|                                       |
|---------------------------------------|
| Example of a Data file titled “colon” |
|                                       |
| Treatment  Stratum   Time  Status     |
|         1        2   1521       1     |
|         1        1   1537       0     |
|         0        2    963       1     |
|         1        2    293       1     |
|         0        2    659       1     |
|         1        2   1767       1     |
|         0        1   1865       0     |
|         1        1   1792       0     |
|         1        1   1841       1     |
|         0        1   1845       1     |
|         0        1   1888       1     |
|         0        1   1701       1     |
|         1        1    887       1     |
|         0        1   1918       1     |
|         1        1   1822       1     |
|         1        1   1638       1     |
|         0        1   1772       1     |
|         0        2    384       1     |
|         0        2    218       1     |
|         1        1   1775       1     |
|         1        1   1526       1     |
|         0        1   1745       1     |
|         0        1   1788       1     |
|         1        1   1387       1     |

## **RESULTS**

### **Result file**

|                                                                                              |
|----------------------------------------------------------------------------------------------|
| Example of a result file                                                                     |
| `Name   Parameter Estimate    Standard Error    Lower       Upper      Chi-Square   P-Value` |
| `Wald   -0.52174              0.26742          -1.045885    0.002399   3.8064       0.0511`  |
| `Score  -0.52174              0.26742          -1.040119    0.003365   3.8926       0.0485`  |
| `Peto   -0.51503              0.26104          -1.026662    0.003395   3.8926       0.0485`  |

## **EXAMPLES**

### **Unstratified Analysis (Stratum not specified)**

The following SAS statement is implemented:

> %HazardRatio(Data = colon, Time = Time, Status = Status, Stratum =, Treatment = Treatment, Alpha = 0.05, Accuracy = 0.000001);

The results are stored in the file with the default name of “HR_CI” as follows:

|                                                                                              |
|----------------------------------------------------------------------------------------------|
| Result file “HR_CI” for Unstratified Analysis on “Colon” data                                |
| `Name   Parameter Estimate    Standard Error    Lower       Upper      Chi-Square   P-Value` |
| `Wald   -0.52174              0.26742          -1.045885    0.002399   3.8064       0.0511`  |
| `Score  -0.52174              0.26742          -1.040119    0.003365   3.8926       0.0485`  |
| `Peto   -0.51503              0.26104          -1.026662    0.003395   3.8926       0.0485`  |

The results for the three methods are provided in three rows. The columns “Parameter Estimate” and “Standard Error” provide the point estimate and standard error for the log hazard ratio. The MPLE is used as the estimate of the log hazard ratio for the score method. The columns “Lower” and “Upper” show the lower and upper limits of the confidence interval. The test statistic and p-value are stored in the columns “Chi-Square” and “P_value”.

### **Stratified Analysis (Stratum specified):**

The following SAS statement is implemented:

> %HazardRatio(Data = colon, Time = Time, Status = Status, Stratum = Stratum, Treatment = Treatment, Alpha = 0.05, Accuracy = 0.000001);

The results are stored in the file with the default name of “HR_CI” as follows:

|                                                                                               |
|-----------------------------------------------------------------------------------------------|
| Result file “HR_CI” for Unstratified Analysis on “Colon” data                                 |
| `Name    Parameter Estimate   Standard Error    Lower        Upper      Chi-Square   P-Value` |
| `Wald   -0.63974              0.27689          -1.182442    -0.097043   5.3381       0.0209`  |
| `Score  -0.63974              0.27689          -1.176296    -0.103231   5.5148       0.0189`  |
| `Score  -0.62758              0.26724          -1.151358    -0.103794   5.5148       0.0189`  |

## **REFERENCES**

Peto, R., Pike, M. C., Armitage, P., Breslow, N. E., Cox, D. R., Howard, S. V., Mantel, N., McPherson, K., Peto, J., and Smith, P. G. (1977). Design and analysis of randomized clinical trials requiring prolonged observation of each patient. II. analysis and examples. British Journal of Cancer 35, 1-39.

Lin, D. Y., Dai, L., Cheng, G., and Sailer, M. O. (2016). On confidence intervals for the hazard ratio in randomized clinical trials. Biometrics.

## **DOWNLOAD**

#### **HazardRatio SAS Program \[updated Feb 17, 2016\]**

SAS HazardRatio **»**[ HazardRatio.zip](http://dlin.web.unc.edu/wp-content/uploads/sites/1568/2016/02/HazardRatio.zip)

#### **Documentation \[updated Feb 17, 2016\]**

Documentation**»** [HazardRatio_Documentation.docx](http://dlin.web.unc.edu/wp-content/uploads/sites/1568/2016/02/HazardRatio.docx)

#### **Example files \[updated July 5, 2012\]**

“Colon” Data **»** [colon.xlsx](http://dlin.web.unc.edu/wp-content/uploads/sites/1568/2016/01/colon.xlsx)

## **VERSION HISTORY**

| Version | Date                    | Description                                            |
|:--------|:------------------------|:-------------------------------------------------------|
|   1.0   |               Feb. 2016 |                                First version released. |
