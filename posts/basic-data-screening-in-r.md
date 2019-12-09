title: A Function for Basic Data Screening in R
slug: basic-data-screening-in-r
date: 2015-03-26 02:42:48 UTC
type: text
tags: programming, research, sport science, statistics
category: Programming
status: published

Note: _I'm using my dissertation as an excuse to learn R. I'm going to put out some of my code for others to peruse/use/improve/correct. Hopefully we can all learn a little bit from it. If you do use it or change it, please let me know in the comments what you did so I can learn too!_

Most statistics that you are going to run rely on a number of assumptions about your data. One of the most frequent assumptions is that your data is normally distributed. The function I've provided below will output into a text file (and the R terminal): basic summary statistics, Skewness, Kurtosis, and the Shapiro-Wilks Test for Normality. The function will also output a histogram, density graph, box plot, and Q-Q plot so that you can really a really good idea of what the 'shape' of your data look like. Just like the [reliability function][1], you will need to load the 'psych' package to use this.

```R
datascreening <- function (x) {

pdf(paste(deparse(substitute(x)), ".pdf"))
sink("screeningcalcs.txt", append=TRUE, split=TRUE)

split.screen(figs=c(2,3))

screen(1)
hist(x, xlab = deparse(substitute(x)))
screen(2)
plot(density(x))
screen(3)
boxplot(x, xlab = deparse(substitute(x)))
screen(4)
qqnorm(x)

summary <- summary(x)
skew <- skew(x)
kurtosis <- kurtosi(x)
shapiro <- shapiro.test(x)

cat(paste("############################################################", "\n"))
cat(paste("SCREENING RESULTS FOR VARIABLE", deparse(substitute(x)), "\n", "\n"))
print(summary)
cat(paste("\n", "\n", "Skewness:", skew))
cat(paste("\n", "\n", "Kurtosis:", kurtosis))
cat(paste("\n", "\n", "Shapiro-Wilks test for normality (want >0.05):", shapiro$p.value,"\n", "\n", "\n", "\n"))

close.screen(all=TRUE)
dev.off()
sink()

}
```

 [1]: http://georgebeckham.com/2015/test-retest-reliability-in-r/