# Regression: a Analysis Based on Box Office Data in China
## Rao Sihang  
### Data Introduction and Processing
  In order to investigate the correlation between film quality and economic perfomance, I collected box offices data and comment scores of more than 400 Chinese films released during 2014-2017. Because box offices I collected are nominal data, which are related to inflation. What’s more, the economic revenue of a flim is restricted by the scale of film market.For example, due to the development of movie industry, the box office of a film showed in 2017 must be higher than that of its counterpart with the similar quality and comment. In order to let those data comparable, I
adjusted the box office.
  It is apparently that Chinese films tend to issue on holidays and festivals and we want to know whether issuing movies on special days contribute to box office. First, special days are defined as Spring Festival, May Day, National Day and summer vacation(June to July). Then all movies in the sample are divided into two groups: a group including those films with special schedule and the other one including remainder.

### Global Regression
  At first, I plotted those data by schedule and use linear lines to fit those data. From the fitting line, we could see there is a significant different between these two groups. with similar quality( same comment scores), a special time contributes positively to economic performance, which is intuitive. During the holiday, people are more likely to have leisure time and go to the cinema. 
  
```R
  film<-read.table("film_box.csv",header=TRUE,sep=",")
  attach(film)
  library(ggplot2)
  film$Time<-factor(film$special,levels=c(0,1),labels=c("ordinary time","special time"))
  ggplot(data=film,aes(x=score,y=lnbox,shape=Time,color=Time))+geom_point()+geom_smooth(method=lm)
```
 ![figure]()
 
 ### Regression Splines
 From my perspective, this advantages for films with special schedule might depend on the film quality.For extremely terrible or extremely great movies, the impact of sechdule tend to be smaller, but for films with modest comments, this influence tends to be more significant. Based on this hypothesis, I think it is necessary to do a spline regression. 
