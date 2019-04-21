# Model Selection and Regularization
## Rao Sihang  
### Data Introduction
 The database I choose is Chinese General Social Survey(CGSS),and I focus on the cross-section data in 201
 In the context of modern society, which is featured by fast-paced lifestyle and Internet-based life, a large number of people are intrigued by virtual world and refused to socialize with others.Due to this hot-discussed phenomena, I wanted to investigate the fatcors impacting a person's preference on sociality. The dependent variable came from the question "Do you often socializa during your free time?", which was an ordered discrete variable(1-5,1 denotes never and 5 denotes frequently). Firstly, I picked up is 52 independent variable, including the basic information of the person and the household(age, gender, nationality,marriage, religion, individual income and so on), the frequency involving in entertainment( shopping, watching TV, going to cinema...), and the personal opinion on relationship and social morality.(More detail information can be seen in the dta.file) By the way, except for income data, most of the independent variables are ordered discrete variables.
 
 
 ### A logistics regression on all independent variables
 ```stata
 ologit free_socialization gender age nation fre_religion highest_edu yearly_income politics health_condition fre_depress hokou houkou_register main_media leisure_tv leisure_film leisure_shopping leisure_reading leisure_culturalactivity leisure_partyrelatives leisure_partyfriends leisure_music leisure_exercise leisure_sportsgame leisure_surf free_learning free_studying fre_socialization_neighbor fre_socialization_friend leave_home trustable_people life_happiness m_career_f_family morethan_onehour_labor weekly_work_time experience job_condition hh_yearly_income hh_economic_level marriage spouse_edu spouse_yearly_inc secono_level connect_friends_family trust_strangers intense_working connectwithbadpeople life_leisure tv_rest moral_satisfication relationship_satisfication indifferent dishonest selfish utilitarian
```

 
   
  
