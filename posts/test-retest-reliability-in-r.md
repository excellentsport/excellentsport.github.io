title: A Function for Test-Retest Reliability for Two Variables in R
slug: test-retest-reliability-in-r
date: 2015-03-01 19:30:18 UTC
type: text
tags: programming, research, sport science
category: Programming
status: published

Note: _I'm using my dissertation as an excuse to learn R. I'm going to put out some of my code for others to peruse/use/improve/correct. Hopefully we can all learn a little bit from it. If you do use it or change it, please let me know in the comments what you did so I can learn too!_

I have a lot of variables to dig through for data collection, and of course, it is vastly important that I am able to check the test re-test reliability of the data. In one of my dissertation studies, I have two trials from a test that I plan to average together. However, I still need to report the within-session test-retest reliability of that test. Here's a short function I wrote to let me do that. This function creates a scatterplot, calculates ICCs, runs a paired t-test, calculates Typical Error, and Relative Typical Error. The function then outputs the results into a text file and the scatterplot as a PDF into your working directory.Once it is loaded,I just have to use "reliabilitypair(var1a, var1b)" to call the function (substituting var1a and var1b for whatever two data columns I plan to compare). Note that it does require you to have the "psych" package installed and loaded to run the ICCs.

_3/7/15 Update: After some tweaking for a bit, I think I have a more effective set of code. I also changed out TE_Rel for CV as outlined by [Hopkins][1], and output only the 3k ICC. The first block of code creates a .csv file with appropriate headers for the reliability output. The rest of the code store the function in R's workspace for you to call up later.Run the entirety of the code below once, then you can call the function as outlined above, as many times as you like. The outputs of interest will be put into the .csv table as new rows. Each time you call the function, a new row will be added to the .csv. Very handy for running through multiple pairs of variables._

```
#creates a csv file with headers
reliabilitydata <- data.frame(setNames(replicate(7,numeric(0), simplify = F), c("Variable", "ICC", "ICC LL", "ICC UL", "TTEST_P", "TE","CV"))) write.table(reliabilitydata, file = "reliabilityresults.csv", append=TRUE, sep=",",col.names=TRUE,row.names=False)
  
reliabilitypair <- function(x, y) {

plot <- plot(x, y, main= paste(c(deparse(substitute(x)), "X",
deparse(substitute(y))), sep = " "),xlab = deparse(substitute(x)),
ylab=deparse(substitute(y)))  ICC <- ICC(cbind(x, y), alpha=0.05)  ttest <- t.test(x,y,paired = TRUE)

xydiff <- (y-x)  xydifflog <- (log(y)-log(x))  TE <- SD(xydiff)/sqrt(2)  CV <- SD(xydifflog)/sqrt(2)

reliabilityvalues <- matrix(c(deparse(substitute(x)),ICC$results[6,2],  ICC$results[6,7],ICC$results[6,8],ttest$p.value,TE,CV),   nrow=1,ncol=7)

reliabilityvalues <-as.data.frame(reliabilityvalues)
  
write.table(reliabilityvalues, file = "reliabilityresults.csv", append=TRUE, sep=",",col.names=FALSE,row.names=FALSE)
  
dev.off()
}
```

[1]: http://www.sportsci.org/resource/stats/relycalc.html