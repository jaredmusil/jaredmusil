+++
title = "Agent Logo"
date = 2017-10-19T23:50:10-05:00
draft = false

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []
categories = []

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
[header]
image = ""
caption = ""
+++

The story of the State Farm agent is one of the greatest business partnership of the past decade. Almost half of our customers interact exclusively with our knowlegable and dedicated agents. As a member of systems, I often have a hard time wrapping my head around exactly how far reaching our agency network is. So to peak my curiousity I dicided to scrape some agent data from statefarm.com and plot the corporate logo with each data point corresponding to a single agent. I hope you enjoy it as much as I did creating it.

```r
# clear workspace
rm(list = ls())

# load libraries
library(EBImage)

set.seed(2017)

# scrape agent data from statefarm.com
# TODO
agent_count = 10000

# load the SF logo, save the rgb values:
img = readImage("statefarm.png")
img_sf = img[,,1:3]

# functions for color simplification
number_to_letter = function(x1) {
  ref_data = data.frame(num = 10:15, let = LETTERS[1:6])
  out = as.character(x1)
  if (x1 %in% 10:15) {
    out = as.character(ref_data$let[which(ref_data$num == x1)])
  }
  return(out)
}

rgb_func = function(vec) {
  #vec = a six character triple octet of color intensities.

  r1 = floor(255 * vec[1])
  g1 = floor(255 * vec[2])
  b1 = floor(255 * vec[3])

  x1 = r1 %/% 16
  x2 = r1 %%  16
  x3 = g1 %/% 16
  x4 = g1 %%  16
  x5 = b1 %/% 16
  x6 = b1 %%  16

  x1 = number_to_letter(x1)
  x2 = number_to_letter(x2)
  x3 = number_to_letter(x3)
  x4 = number_to_letter(x4)
  x5 = number_to_letter(x5)
  x6 = number_to_letter(x6)

  out = paste("#", x1, x2, x3, x4, x5, x6, sep = "")
  return(out)
}

im_func_1 = function(image, k_cols = 5, sample_value = 3000) {
  #create a dataframe
  test_matrix = matrix(image, ncol = 3)
  df = data.frame(test_matrix)
  colnames(df) = c("r", "g", "b")
  df$y = rep(1:dim(image)[1], dim(image)[2])
  df$x = rep(1:dim(image)[2], each = dim(image)[1])

  samp_index = sample(1:nrow(df), sample_value)
  work_sub = df[samp_index,]

  # extract colors
  k2 = kmeans(work_sub[ ,1:3], k_cols)

  # adding centers back
  fit_test = fitted(k2)

  work_sub$r.pred = fit_test[ ,1]
  work_sub$g.pred = fit_test[ ,2]
  work_sub$b.pred = fit_test[ ,3]

  return(work_sub)
}


add_cols = function(data){
  apply(data, 1, rgb_func)
}

# general plotting function
plot_func = function(data){

  #Uncomment out this to save a high resolution .png photo
  #png("agents.png", width = 10, height = 10, units = "in", res = 1200)

  windows.options(width=10, height=10)

  # assumes data has colums x, ym cols
  plot(
    data$y,
    max(data$x) - data$x,
    col = data$cols,
    main = "A point for each State Farm Agent",
    xaxt = 'n',
    yaxt = "n",
    xlab = "",
    ylab = "",
    cex.lab = 1.5,
    cex.axis = 1.5,
    cex.main = 1.5,
    cex.sub = 1.5,
    fin = c(5, 5),
    asp = 1
  )

  identify(data$x, max(data$x) - data$x, labels(row.names(1:10000)))
}

#### simplify colors; sample n points ###

temp = im_func_1(img_sf, sample_value = 25000, k = 12)
temp$cols = add_cols(temp[,6:8])

final = temp[sample(1:nrow(temp), agent_count),]

#### generate plot ####

plot_func(final)

```

The full resolution image can be downloaded [here]("agents.png"), and the script [here]("agents.R").
