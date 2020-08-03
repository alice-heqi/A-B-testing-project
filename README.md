# A-B-testing-final-project

## Introduction
This project is the final project of Udacity A/B testing course. The course link is here: [course](https://classroom.udacity.com/courses/ud257/lessons/4018018619/concepts/41149386080923)

The python code that processing this project is here [notebook](https://github.com/alice-heqi/A-B-testing-project/blob/master/Udacity_A_B_testing_Final_project.ipynb)

## Project background

### Experiment Overview: Free Trial Screener

At the time of this experiment, Udacity courses currently have two options on the course overview page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.
In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot shows what the experiment looks like.
The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.
The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

### Metric Choice

- Number of cookies: That is, number of unique cookies to view the course overview page. (dmin=3000)
- Number of user-ids: That is, number of users who enroll in the free trial. (dmin=50)
- Number of clicks: That is, number of unique cookies to click the "Start free trial" button (which happens before the free trial screener is trigger). (dmin=240)
- Click-through-probability: That is, number of unique cookies to click the "Start free trial" button divided by number of unique cookies to view the course overview page. (dmin=0.01)
- Gross conversion: That is, number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the "Start free trial" button. (dmin= 0.01)
- Retention: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (dmin=0.01)
- Net conversion: That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the "Start free trial" button. (dmin= 0.0075)

**Step 1: Choosing Invariant Metrics** 

    - Invariant metrics is the data that won’t be impacted by the tested feature change. They are used to check if there is anything wrong with experiment setup or any abnormal data collection
    
 -	Number of cookies
 -  Number of clicks
 - Click-through-probability

**Step 2: Choosing Evaluation Metrics** 

    - metric that will change if the test hypothesis is true, in other words, the evaluation metric is used for checking if the data before and after experiment are different and whether the difference is statistical and practical significant
    
 - Gross conversion
 - Retention
 - Net conversion

**Step 3: Calculating analytic standardard deviation** 

    - before getting real experiment data, we need make analytic estimation such as the involved metrics’ standard deviation
    
 _ _sample size to estimate the standard deviation: 5000 pageviews_ _

  - std of gross_conversion baseline 0.0202
  - std of retention baseline 0.0549
  - std of net_conversion baseline 0.0156

**Step 4: Sizing** 

    - before starting the experiment, we need confirm the sample data size needed and the duration and exposure
    
- confirm sample size: calculated with this link:[sample size calculator](https://www.evanmiller.org/ab-testing/sample-size.html)

 _ _---alph: 0.95
 ---beta: 0.2_ _

- because the unit of diversion in this project is unique cookies that review the course page, so when calculating the pageviews needed for each of the evaluation metric, the unit of analysis(the denominator) should be converted to equivalent to the sample pageviews.

  - pageview needed for gross_conversion 645875.0
  - pageview needed for retention 4741212
  - pageview needed for net_conversion 685325.0

- Duration and exposure

  - The maximum pageview numbers needed to cover all the three metrics is 4741212, however it will take too much time to collect such big volume pagevies, so the next best number of pageview is 685325.0

**Step 5: Sanity Check** 

- invariant metric: Number of cookies, check if the data size is equally assigned to control group and experiment group

  - probability of pagevies in control group 0.5006 
  - upper bound of pageview probability in control group 0.5012
  - lower bound of pageview probability in control group 0.4988
  - sanity check of pageviews: pass
 
- invariant metric: Number of clicks, check if the data size is equally in control group and experiment group
 
  - probability of clicks in control group 0.5005
  - upper bound of clicks probability in control group 0.5041
  - lower bound of clicks probability in control group 0.4959
  - sanity check of clicks: pass

- invariant metric: Click-through-probability, check if the this probability is equal in control group and experiment group

  - difference of ctp between control group and experiment group 0.0001
  - upper bound of confidence interval 0.0013
  - lower bound of confidence interval -0.0013
  - sanity check of click through probability: pass

**Step 6: Effect size tests** 

- To set the confidence level in this project, since the three metrics I test are highly correlated, "assume independence" method is more appropriate. That is, for each individual metric, calculate at 95% confidence level.

- evaluation metric: Gross conversion

  - difference of gross_conversion between control group and experiment group: -0.020554874580361565
  - upper bound of gross_conversion confidence interval: -0.012
  - lower bound of gross_conversion confidence interval: -0.0291
  - CI did not include 0,change of gross_conversion stastistic significant: True
  - CI did includes d_min_gross, change of gross_conversion practical significant: True
 
- evaluation metric: Retention

  - difference of retention between control group and experiment group: 0.031094804707142765
  - upper bound of retention difference confidence interval: 0.0541
  - lower bound of retention difference confidence interval: 0.0081
  - CI did not include 0,change of retention stastistic significant: True
  - CI includes d_min_retention,change of gross_conversion practical significant: False
 
- evaluation metric: Net conversion

  - difference of net_conversion between control group and experiment group: -0.0048737226745441675
  - upper bound of net_conversion confidence interval: 0.0541
  - lower bound of net_conversion confidence interval: 0.0081
  - CI did includes 0, change of net_conversion stastistic significant: False
  - CI did includes d_min_retention, change of net_conversion practical significant: False
 
 **Step 7: sign tests** 
 
- evaluation metric: Gross conversion
 
  - p-value: 1.0843941709026694e-06 , Statistically Significant: True
 
- evaluation metric: Retention

  - p-value: 0.09887174959294497 , Statistically Significant: False
 
- evaluation metric: Net conversion

  - p-value: 0.007632078602910038 , Statistically Significant: True
 
### Conclusion

Of all the test results, only the first metric: gross_conversion are both statistical significant and practical significant, the change of the second metric: retention is statistical significant, but not practical significant, and the third metric:net_conversion are both not statistical significant and practical significant. I think the difference between control and experiment group didn't prove the hypothesis of this test, therefore my recommendation is: not launch. 
 







