# Regression: a Analysis Based on Box Office Data in China
## Rao Sihang  
### Data Introduction and Processing
  In order to investigate the correlation between film quality and economic perfomance, I collected box offices data and comment scores of more than 400 Chinese films released during 2014-2017. Because box offices I collected are nominal data, which are related to inflation. What’s more, the economic revenue of a flim is restricted by the scale of film market.For example, due to the development of movie industry, the box office of a film showed in 2017 must be higher than that of its counterpart with the similar quality and comment. In order to let those data comparable, I
adjusted the box office according to release time.And I used logarithm form of box office.
It is apparently that Chinese films tend to issue on holidays and festivals and we want to know whether issuing movies on special days contribute to box office. First, special days are defined as Spring Festival, May Day, National Day and summer vacation(June to July). Then all movies in the sample are divided into two groups: a group including those films with special schedule and the other one including remainder.

### Global Regression
  At first, I plotted those data by schedule and use linear lines to fit those data. From the fitting line, we could see there is a significant different between these two groups. with similar quality( same comment scores), a special time contributes positively to economic performance, which is intuitive. During the holiday, people are more likely to have leisure time and go to the cinema. 
  
```R
  film<-read.table("film_box.csv",header=TRUE,sep=",")
  attach(film)
  library(ggplot2)
  film$Time<-factor(film$special,levels=c(0,1),labels=c("ordinary time","special time"))
  ggplot(data=film,aes(x=score,y=lnbox1,shape=Time,color=Time))+geom_point()+geom_smooth(method=lm)
```
 ![global regression](https://github.com/rshhh/rshhh.github.io/blob/master/figure/figure1.png)

 
 ### Regression Splines
From my perspective, this advantages for films with special schedule might depend on the film quality.For extremely terrible or extremely great movies, the impact of sechdule tend to be smaller, but for films with modest comments, this influence tends to be more significant. Based on this hypothesis, I think it is necessary to do a spline regression. Using 40 and 70 points as two thresholds, I divided those data into three groups. The blue color lines are cubic spline regression lines and orange ones are natural cubic spline regression with linear function beyond boundary knots. And the   darker points and lines represent ordinary time groups.

```R
library(zoo)
library(splines)
library(ggplot2)
film<-read.table("film_box.csv",header=TRUE,sep=",")
attach(film)
film$Time<-factor(film$special,levels=c(0,1),labels=c("ordinary time","special time"))
scorelims<-range(score)
score.grid<-seq(from=scorelims[1],to=scorelims[2])
film$color[film$Time=="ordinary time"]<-"black"
film$color[film$Time=="special time"]<-"darkgrey"
plot(score,lnbox1,xlim=scorelims,cex=.5,col=film$color,pch=16,pin=c(3,4),xlab="",ylab="")
fit1 <- lm(lnbox1~bs(score, knots = c(40,70),df=3),data=film,subset=(Time == "ordinary time"))
pred1 <- predict(fit1,newdata=list(score=score.grid),se=TRUE)
se.bands <- cbind(pred1$fit + 2*pred1$se.fit, pred1$fit - 2*pred1$se.fit)
lines(score.grid,pred1$fit,lwd=2,lty=1,col="darkblue")
matlines(score.grid,se.bands,lwd=1,lty=3,col="darkblue")
fit2 <- lm(lnbox1~bs(score, knots = c(40,70), df = 3),data=film,subset=(Time == "special time"))
pred2 <- predict(fit2,newdata=list(score=score.grid),se=TRUE)
se2.bands <- cbind(pred2$fit + 2*pred2$se.fit, pred2$fit - 2*pred2$se.fit)
lines(score.grid,pred2$fit,lwd=2,lty=1,col="blue")
matlines(score.grid,se2.bands,lwd=1,lty=3,col="blue")
fit3 <- lm(lnbox1~ns(score, knots = c(40,70)),data=film,subset=(Time == "ordinary time"))
pred3 <- predict(fit3,newdata=list(score=score.grid),se=TRUE)
se3.bands <- cbind(pred3$fit + 2*pred3$se.fit, pred3$fit - 2*pred3$se.fit)
lines(score.grid,pred3$fit,lwd=2,lty=1,col="darkorange")
matlines(score.grid,se3.bands,lwd=1,lty=3,col="darkorange")
fit4 <- lm(lnbox1~ns(score, knots = c(40,70)),data=film,subset=(Time == "special time"))
pred4 <- predict(fit4,newdata=list(score=score.grid),se=TRUE)
se4.bands <- cbind(pred4$fit + 2*pred4$se.fit, pred4$fit - 2*pred4$se.fit)
lines(score.grid,pred4$fit,lwd=2,lty=1,col="orange")
matlines(score.grid,se4.bands,lwd=1,lty=3,col="orange")
abline(v=c(40,70),lty=3)
legend("topright",col=c("darkblue","blue","darkorange","orange"),lwd=0.5,cex=0.5,legend=c("Cubic Spline/ordinarytime","Cubic Spline/specialtime","Natural Cubic Spline/ordinarytime","Natural Cubic Spline/specialtime"))
title("Cubic and Natural Cubic regression",xlab="Comment Score",ylab="log box office")    
```
![Cubic and Natural Cubic Regression](https://github.com/rshhh/rshhh.github.io/blob/master/figure/figure2.png)
Based on cubic regression lines, the advantages of special schedule is much smaller for those movies with extreme quality,but the natural cubic regression lines tell us this advantages doesn't depends on comment scores. Due to the limited observations with extreme low and high scores, even though the natural cubic regression is much stable than the other on,but  I still don't consider that those fitting lines can reliably illustarte the difference of this advantages for different qualities films. 
There are other essential factors having great influence on box office, such as popularity of actors, types and so on.In order to investigate this problem, I still need other varibles.
