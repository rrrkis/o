---
title: Графики в R
---
#template

* Вообще, по неизвестной причине графики не стандартной библиотеки рисуются не всегда, если их писать, не оборачивая в `print`, i.e. `print(histogram(x))`.
* Сохранение делается через обертку в 
```R
jpeg(plot_stable_name, width = 1200, height = 900)
...
dev.off()
```


## Несколько линий на одной картинке (ggplot)

```R 
ggplot() +
    geom_smooth(data = df %>% filter(Diet == "Diet 4"),
        aes(x = Time, y = weight), fill = "lightcyan3", colour = "blue", size = 1)
    + geom_smooth(data = df %>% filter(Diet == "Diet 1"),
        aes(x = Time, y = weight), fill = "darksalmon", colour = "red", size = 1)
    + geom_smooth(data = df %>% filter(Diet == "Diet 3"),
        aes(x = Time, y = weight), fill = "palegreen", colour = "green", size = 1)
    + geom_smooth(data = df %>% filter(Diet == "Diet 2"),
        aes(x = Time, y = weight), fill = "lightgoldenrod3", colour = "orange", size = 1)
    + labs(title = "Chickens Weight at Different Ages",
        x = "Age (days)",
        y = "Weight (gm)")
    + theme(text = element_text(size = 18))
```

```R
ggplot()
    + geom_density(data = df %>% filter((Diet == "Diet 3") & (Time >= 10) & (Time <= 16)), aes(x = weight), fill ="palegreen", alpha = 0.5)
    + geom_density(data = df %>% filter((Diet == "Diet 4") & (Time >= 10) & (Time <= 16)), aes(x = weight), fill ="darksalmon", alpha = 0.5)
    + labs(title = "Density of Weights, Diets 3 and 4 (Days 10-16)",
        x = "Weight (gm)",
        y = "Density")
    + theme(text = element_text(size = 18))
)
```

## Несколько графов на одной картинке (lattice)

```R
bwplot(weight ~ Time | Diet,
    data = df %>% filter(Time %in% c(0, 10, 14, 21)) %>% mutate(Time = paste(Time)),
    type = c("p", "g", "smooth"),
    xlab = list("Time (days)", fontsize = 18),
    ylab = list("Weight (gm)", fontsize = 18),
    main = list("Distrubition of Masses, with Times and Diets", fontsize = 20),
    scales = list(cex = c(1.2, 1.2), alternating = 2)
)
```

```R
xyplot(Speed ~ Time | Diet,
    data = df %>% filter(Time != min(df$Time)),
    type = c("p", "g", "smooth"),
    xlab = list("Time (day)", fontsize = 16),
    ylab = list("Mass Increase Speed (gm/day)", fontsize = 16),
    main = list("Chickens Mass Increase Speed ", fontsize = 20),
    scales = list(cex = c(1.2, 1.2), alternating = 2)
)
```
