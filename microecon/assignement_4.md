# Model Selection and Regularization
## Rao Sihang  
### Data Introduction
 The database I choose is Chinese General Social Survey(CGSS),and I focus on the cross-section data in 201
 In the context of modern society, which is featured by fast-paced lifestyle and Internet-based life, a large number of people are intrigued by virtual world and refused to socialize with others.Due to this hot-discussed phenomena, I wanted to investigate the fatcors impacting a person's preference on sociality. The dependent variable came from the question "Do you often socializa during your free time?", which was an ordered discrete variable(1-5,1 denotes never and 5 denotes frequently). Firstly, I picked up is 52 independent variable, including the basic information of the person and the household(age, gender, nationality,marriage, religion, individual income and so on), the frequency involving in entertainment( shopping, watching TV, going to cinema...), and the personal opinion on relationship and social morality.(More detail information can be seen in the dta.file) By the way, except for income data, most of the independent variables are ordered discrete variables.
 
 
 ### A logistics regression on all independent variables with an ordered discrete dependent variable
 ```stata
 ologit free_socialization gender age nation fre_religion highest_edu yearly_income politics health_condition fre_depress hokou houkou_register main_media leisure_tv leisure_film leisure_shopping leisure_reading leisure_culturalactivity leisure_partyrelatives leisure_partyfriends leisure_music leisure_exercise leisure_sportsgame leisure_surf free_learning free_studying fre_socialization_neighbor fre_socialization_friend leave_home trustable_people life_happiness m_career_f_family morethan_onehour_labor weekly_work_time experience job_condition hh_yearly_income hh_economic_level marriage spouse_edu spouse_yearly_inc secono_level connect_friends_family trust_strangers intense_working connectwithbadpeople life_leisure tv_rest moral_satisfication relationship_satisfication indifferent dishonest selfish utilitarian
```
### Use Lasso to do L1 regularization
#### Plot the path of lambda
```stata
ssc install lassopack
lasso2 free_socialization gender age nation fre_religion highest_edu yearly_income politics health_condition fre_depress hokou houkou_register main_media leisure_tv leisure_film leisure_shopping leisure_reading leisure_culturalactivity leisure_partyrelatives leisure_partyfriends leisure_music leisure_exercise leisure_sportsgame leisure_surf free_learning free_studying fre_socialization_neighbor fre_socialization_friend leave_home trustable_people life_happiness m_career_f_family morethan_onehour_labor weekly_work_time experience job_condition hh_yearly_income hh_economic_level marriage spouse_edu spouse_yearly_inc secono_level connect_friends_family trust_strangers intense_working connectwithbadpeople life_leisure tv_rest moral_satisfication relationship_satisfication indifferent dishonest selfish utilitarian,plotpath(lambda)
``` 
![pathlambda](https://github.com/rshhh/rshhh.github.io/blob/master/figure/path(lambada).png)

#### k-fold cross validation
```stata
cvlasso free_socialization gender age nation fre_religion highest_edu yearly_income politics health_condition fre_depress hokou houkou_register main_media leisure_tv leisure_film leisure_shopping leisure_reading leisure_culturalactivity leisure_partyrelatives leisure_partyfriends leisure_music leisure_exercise leisure_sportsgame leisure_surf free_learning free_studying fre_socialization_neighbor fre_socialization_friend leave_home trustable_people life_happiness m_career_f_family morethan_onehour_labor weekly_work_time experience job_condition hh_yearly_income hh_economic_level marriage spouse_edu spouse_yearly_inc secono_level connect_friends_family trust_strangers intense_working connectwithbadpeople life_leisure tv_rest moral_satisfication relationship_satisfication indifferent dishonest selfish utilitarian,lopt seed(123)
````
![Lasso results](https://github.com/rshhh/rshhh.github.io/blob/master/figure/Lasso.png)
  
 ### Do logistics regression on independent variables chosen by Lasso
 ```
ologit free_socialization yearly_income politics hokou leisure_tv leisure_film leisure_shopping leisure_partyrelatives leisure_partyfriends leave_home life_happiness weekly_work_time connect_friends_family connectwithbadpeople  relationship_satisfication
```
![logistics](https://github.com/rshhh/rshhh.github.io/blob/master/figure/ologit.png)

### A brief conclusion
 According to the results, job-related issues( income, working time ) are closely related to person's sociality, which is justifiable, because the time and energy are limited. Besides, we can know the happiness level is positively related to sociality, which might mean more time spending with friends and participating in activities tends to improve one's mood. Of course, from the sign and significance level of those variables, we can find some evidence or reference in accordance with our intuitive. But 
