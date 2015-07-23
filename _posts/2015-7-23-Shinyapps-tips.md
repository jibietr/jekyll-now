---
layout: post
title: Shinyapps.io tips
---

I have gone through inumerable issues trying to use shinyapps.io to deploy an Rmarkdown
web app that would work fine with in my local environtment but would fail to work 
once uploaded to the Rstudio servers. While [Shinyapps.io's get started guide](http://shiny.rstudio.com/articles/shinyapps.html)
is great for a quick start, it is quite limited in terms of documenting basic
useful practices that can save a lot of time. This is my list:


tip #1: use the `appFile` parameter in `shinyapps::deployApp` 

by default, deployApp uploads all the files in your directory, so unless
you have one single `.Rmd` it can get messy. I have also experienced
`connection` errors because R was unable to locate data that was not 
directly available in the current directory. A solution is the following:

```R
app.files <- list('main_file.Rmd',
	  'data_path/main_data.csv')
shinyapps::deployApp(appFiles=app.files,appName='my_app_name')
```

tip #2: `showLogs` is your friend

this is quite basic, but I only got to knew about it when I 
started getting into complex errors. Use it after deploying
to monitor the deployment result:

```R
shinyapps::showLogsp(appName='my_app_name')
```

tip #3: load the libraries you are using

I run into issues with libraries that were not available in shinyapps.io
at running time, most likely to broken dependencies of some packages.
A way to avoid this is to explicitly load all the libraries
that you are using in your local session. You use `showInfo()`
to check what are the libraries under `other attached packages` and
use `require` to explicitly load them in your `.Rmd` file.


tip #4: options(mc.cores=1) 



