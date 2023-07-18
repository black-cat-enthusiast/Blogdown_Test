---
title: "Codes to Create Basic Graphs Using ggplot2"
author: "Jennet Baumbach"
date: "2023-04-05"
output:
  html_document: default
  pdf_document: default
---



# First thing's first: Load packages

```r
library(tidyverse)
library(reshape2)
library(ggpubr)
library(readr)
```

# Get Data: EB Rats (data from my MA thesis - published [here](https://link.springer.com/article/10.1007/s00213-020-05685-8))

```r
data <- read_csv("EB_Rats_Nicotine_Sensitization.csv")

data$PREhorm = factor(data$PREhorm,# change the PREhorm variable to a factor
                      levels = c(0,1), # With two levels, 0 and 1 (ordered)
                      labels = c("OIL","EB")) # And label those levels, the condition names.

data$CHALhorm = factor(data$CHALhorm, # change the PREhorm variable to a factor
                      levels = c(0,1),
                      labels = c("OIL","EB"))
```

# Bar chart with error bars: 

```r
a <- as.data.frame(data$ID) # Create temp df
a$PREhorm <- data$PREhorm # Attach hormone variables
a$CHALhorm <- data$CHALhorm 
colnames(a) <- c("ID","PREhorm","CHALhorm") # name them

CHAL <- a
CHAL$distance <- data$CHAL

n <- CHAL %>% # Get info about Input's n
  group_by(`PREhorm`) %>%
  summarise(n=n())

means <- CHAL %>% # Get info about Input's mean distance value
  group_by(PREhorm) %>%
  summarise(mean=mean(distance))

sd <- CHAL %>% # Get sd info
  group_by(PREhorm) %>%
  summarise(sd=sd(distance))

se <- sd[-1] # Drop the ID column, because it's problematic in the nest step and is not longer needed.
se <- se / sqrt(n$n-1) # calculate standard error

m_means <- melt(means) # Get data to ggplot format
m_se <- melt(se)
m_means$se <- m_se$value # Bind se values to the means df.

a <- ggplot(m_means, aes(x=PREhorm,y=value,colour=PREhorm,fill=PREhorm))+ # Create bar chart 
  geom_bar(stat = "identity", alpha=0.2)+
  geom_errorbar(aes(x=PREhorm,ymin=value-se,ymax=value+se), width=0.4, size=0.8,alpha=0.8)+
  scale_colour_manual(values=c("#89CFF0","#CD5E77"))+
  scale_fill_manual(values=c("#89CFF0","#CD5E77"))+
  theme_classic()+
  theme(legend.position = "none")+
  theme(plot.title = element_text(hjust=0.5))+
  theme(plot.subtitle = element_text(hjust=0.5))+
  labs(
    x=" ",
    y="distance travelled (cm)",
    title = "Bar chart with Error Bars",
    subtitle = "Useful to show a between-group effect"
  )+
  ylim(0,40000)

a # Check out the result
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

# Add individual points

```r
a + 
  geom_jitter(data=data,aes(x=PREhorm,y=CHAL,shape=PREhorm),size=4,alpha=0.4,width=0.25) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

# Bar Chart with Lines for within-subjects data

```r
lines_data <- data[ ,1:7]
lines_data <- lines_data[-2]
lines_data <- lines_data[-2]

n <- lines_data %>% # Get info about Input's n
  summarise(n=n())

means <- lines_data %>% # Get info about Input's mean distance value
  summarise_at(vars(Hab:IND_1),mean) 

sd <- lines_data %>% # Get sd info
  summarise_at(vars(Hab:IND_1),"sd")

se <- sd[-1] # Drop the ID column, because it's problematic in the nest step and is not longer needed.
se <- se / sqrt(n$n-1) # calculate standard error

m_means <- melt(means) # Get data to ggplot format
m_se <- melt(se)
m_means$se <- m_se$value # Bind se values to the means df.

m_lines_data <- melt(lines_data, id.vars=c("ID","PREhorm","CHALhorm"))

a <- ggplot(m_means, aes(x=variable,y=value))+
  geom_bar(stat="identity", alpha=0.2,colour="black",fill="black",size=1)+
  geom_errorbar(aes(ymin=value-se,ymax=value+se),size=1,width=0.25, alpha=0.8,colour="black")+
  theme_classic()+
  theme(legend.position = c(0,1), legend.justification = c(0,1))+
  theme(plot.title = element_text(hjust=0.5))+
  theme(plot.subtitle = element_text(hjust=0.5))+
  labs(x="",
       y="Distance Travelled (cm)",
       title="Bar chart with individual lines",
       subtitle = "Shows within-subjects effect across time")

a + geom_line(data=m_lines_data, aes(x=variable,y=value,group=ID),alpha=0.8,size=1)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

```r
a + geom_line(data=m_lines_data, aes(x=variable,y=value,group=ID,colour=PREhorm),alpha=0.8,size=1)+
    scale_colour_manual(values=c("#89CFF0","#CD5E77"))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-2.png" width="672" />

# Special Request from Crystal

```r
a <- data[,1:2]
a$PREhorm <- data$PREhorm
a <- a[-2]
b <- data[,8:9]
c <- cbind(a,b)

n <- data %>%
  group_by (PREhorm) %>%
  summarise(n=n())

means <- data %>%
  group_by(PREhorm) %>%
  summarise_at(vars(IND_2:CHAL),"mean")

sds <- data %>%
  group_by(PREhorm) %>%
  summarise_at(vars(IND_1:IND_2),"sd")

se <- sds [,-1]
se <- se / sqrt(n$n-1)

m_means <- melt(means)
m_se <- melt(se)
m_means$se <- m_se$value

a <- ggplot(m_means,aes(x=variable,y=value,colour=PREhorm,fill=PREhorm))+
  scale_colour_manual(values=c("#89CFF0","#CD5E77"))+
  scale_fill_manual(values=c("#89CFF0","#CD5E77"))+
  geom_bar(stat="identity",alpha=0.2,size=1)+
  geom_errorbar(aes(x=variable,ymin=value-se,ymax=value+se),size=1,width=0.25)+
  theme_classic()+
  facet_wrap(~PREhorm)

lines_data <- data[ ,1:5]
lines_data <- lines_data[-2]
lines_data <- lines_data[-2]
lines_data$IND_2 <- data$IND_2
lines_data$CHAL <- data$CHAL
m_lines_data <- melt(lines_data, id.vars=c("ID","PREhorm","CHALhorm"))

b <- a + 
  geom_line(data=m_lines_data, aes(x=variable,y=value,group=ID,colour=PREhorm),alpha=0.8,size=1)+
  theme(plot.title = element_text(hjust=0.5))+
  theme(legend.position = "none")+
  labs(title="EB during induction enables expression of sensitization",
       x=" ",
       y="Distance Travelled (cm)")

ggsave("wi_CHAL.png",b,dpi=300,height=4,width=7)
```

# Line chart acrss days to show within subjects data

```r
data2 <- data
data2 <- data2[-2]
data2 <- data2[-2]
m_data <- melt(data, id.vars=c("ID","PREhorm","CHALhorm"))

n <- data %>% # Get info about Input's n
  group_by(`PREhorm`,`CHALhorm`)%>%
  summarise(n=n())

means <- data %>% # Get info about Input's mean distance value
  group_by(`PREhorm`,`CHALhorm`)%>%
  summarise_at(vars(Hab:CHAL),mean) 

sd <- data %>% # Get sd info
  group_by(`PREhorm`,`CHALhorm`)%>%
  summarise_at(vars(Hab:CHAL),"sd")

se <- sd[-1] # Drop the ID column, because it's problematic in the nest step and is not longer needed.
se <- se[-1]
se <- se / sqrt(n$n-1) # calculate standard error

m_means <- melt(means,id.vars=c("PREhorm","CHALhorm")) # Get data to ggplot format
m_se <- melt(se)
m_means$se <- m_se$value # Bind se values to the means df.
m_means$group <- c(1,2,3,4,1,2,3,4,1,2,3,4,1,2,3,4)

a <- ggplot(m_means,aes(x=variable,y=value,colour=PREhorm,shape=CHALhorm,group=group))+
  geom_point(size=5,alpha=0.8)+
  geom_line(size=2,alpha=0.8)+
  scale_colour_manual(values=c("#89CFF0","#CD5E77"))+
  geom_errorbar(aes(x=variable,ymin=value-se,ymax=value+se),size=2,width=0.25,alpah=0.8)+
  theme_classic()+
  theme(plot.title = element_text(hjust=0.5))+
  theme(plot.subtitle = element_text(hjust=0.5))+
  theme(legend.position = c(0,1), legend.justification = c(0,1))+
  labs(x="",
       y="Distance Travelled (cm)",
       title= "Line chart with means and error bars (SE)",
       subtitle="To show changes in group means across time")

a 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="672" />

# Line chart plus individual points

```r
data2 <- data 
data2 <- data2[-2]
data2 <- data2[-2]
data2$group <- 0

data2 <- data2 %>%
  mutate(group = case_when(
    data2$PREhorm == "OIL" | data2$CHALhorm == "OIL" ~ 1,
    data2$PREhorm == "EB" | data2$CHALhorm == "OIL" ~ 2,
    data2$PREhorm == "OIL" | data2$CHALhorm == "EB" ~ 3,
    data2$PREhorm == "EB" | data2$CHALhorm == "EB" ~4
  ))

m_data <- melt(data2,id.vars=c("ID","group","PREhorm","CHALhorm"))

a + 
  geom_jitter(data=m_data,size=4,alpha=0.5,width=0.25)+
  labs(
    subtitle="Individual points helps showcase the nature of the data"
  )
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-1.png" width="672" />

# Line chart plus individual lines

```r
a + 
  geom_line(data=m_data, aes(x=variable,y=value,group=ID),alpha=0.5)+
  labs(
    subtitle = "Lines showcase individual changes across the paradigm"
  )
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="672" />

# Continuous predictor + Continuous outcome 

```r
ggplot(data, aes(x=Hab,y=CHAL,colour=PREhorm,shape=PREhorm,fill=PREhorm))+
  geom_point(size=5,alpha=0.5)+
  geom_smooth(method="lm",alpha=0.2)+
  scale_colour_manual(values=c("#89CFF0","#CD5E77"))+
  scale_fill_manual(values=c("#89CFF0","#CD5E77"))+
  theme_classic()+
  theme(legend.position = c(0,1), legend.justification = c(0,1))+
  theme(plot.title = element_text(hjust=0.5))+
  theme(plot.subtitle = element_text(hjust=0.5))+
  labs(
    x="Distance travelled during the habituation session",
    y="Distance travelled on the challenge day",
    title = "Scatterplot with Lines of Best fit added for each group",
    subtitle = "Shows the relationship between 2 continuous variables"
  )
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-10-1.png" width="672" />


