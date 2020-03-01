plot# DataVisualization_A1.2-Nightingale
Replication of Florence Nightingale’s Visualization in R

```
data <- read.csv("data.csv")

night_data <- data[,c(1,8:10)]
melted_data <- melt(night_data, "Date")
names(melted_data) <- c("Date", "Cause", "Deaths")
melted_data$Cause <- sub("\\.rate", " ", melted_data$Cause)
melted_data$Regime <- ordered( rep(c(rep('Before', 12), rep('After', 12)), 3), levels=c('Before', 'After'))
night_data <- night_melted

night_subset_unordered_1 <- subset(night_data, Date < as.Date("1855-04-01"))
night_subset_ordered_1 <- night_subset_unordered_1[order(night_subset_unordered_1$Deaths, decreasing=TRUE),]

night_subset_unordered_2 <- subset(night_data, Date >= as.Date("1855-04-01"))
night_subset_ordered_2 <- night_subset_unordered_2[order(night_subset_unordered_2$Deaths, decreasing=TRUE),]

night_data <- rbind(night_subset_ordered_1, night_subset_ordered_2)

plot_1 <- ggplot(night_subset_ordered_1, aes(x = factor(Date), y=Deaths, fill = Cause)) + geom_bar(width = 1, position="identity", stat="identity", color="grey") + scale_y_sqrt() 
plot1 + coord_polar(start=3*pi/2) + ggtitle("East: Reasons for army mortality") + xlab("")

plot2 <- ggplot(night_subset_ordered_2, aes(x = factor(Date), y=Deaths, fill = Cause)) + geom_bar(width = 1, position="identity", stat="identity", color="black") + scale_y_sqrt()
plot2 + coord_polar(start=3*pi/2) +	ggtitle("East: Reasons for army mortality") + xlab("")

plot_combined <- ggplot(night_data, aes(x = factor(Date), y=Deaths, fill = Cause)) + geom_bar(width = 1, position="identity", stat="identity", color="black") +  scale_y_sqrt() + facet_grid(. ~ Regime, scales="free", labeller=label_both)
plot_combined + coord_polar(start=3*pi/2) +	ggtitle("East: Reasons for army mortality") + 	xlab("")
```
