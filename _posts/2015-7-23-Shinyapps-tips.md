---
layout: post
title: Tips using Shinyapps.io 
---

I have gone through inumerable issues trying to use [shinyapps.io](http://www.shinyapps.io/) to deploy an Rmarkdown
web app that would work fine when deploying it locally but would fail to work 
after uploading it to Rstudio's servers. While [Shinyapps.io's get started guide](http://shiny.rstudio.com/articles/shinyapps.html)
is great for a quick start, it is quite limited in terms of documenting basic
useful practices that can save a lot of time. These are some tips:


**tip #1: use the `appFile` parameter in `shinyapps::deployApp`**

by default, deployApp uploads all the files in your directory, so unless
you have one single `.Rmd` things can get messy. I have also experienced
`connection`, because R was unable to locate data that was not 
directly available in the current directory (this error appeared quite inconsistenly). 
One solution is to explicitly mention all the files required by your app:

```R
app.files <- list('main_file.Rmd',
	  'data_path/main_data.csv')
shinyapps::deployApp(appFiles=app.files,appName='my_app_name')
```

this is related what is discussed [here](http://stackoverflow.com/questions/26316192/uploading-csv-file-to-shinyapps-io)


**tip #2: `showLogs` is your friend**

this is quite basic, but I only got to know about it after I 
started getting into erros difficult to debug. Use it after `deployApp`
to monitor the deployment result:

```R
shinyapps::showLogs(appName='my_app_name')
```

**tip #3: `require` all the libraries you need**

I run into issues with libraries that were not available in shinyapps.io
at running time, most likely to broken dependencies of some packages.
A way to avoid this is to explicitly load all the libraries
that you are using in your local session. You use `showInfo()`
to check what are the libraries under `other attached packages`, and 
then use `require` to explicitly load them in your `.Rmd` file.


**tip #4: `options(mc.cores=1)` is magic when using `tm` and `parallel` package**

Again, I found several different errors when using functions from 
the `tm` package that depend on `parallel`. Using `options(mc.cores=1)` solves
the problem in one shot. Some of these issues, like the ones discussed 
[here](http://stackoverflow.com/questions/25069798/r-tm-in-mclapplycontentx-fun-all-scheduled-cores-encountered-errors),
[here](http://stackoverflow.com/questions/26834576/big-text-corpus-breaks-tm-map), or 
[here](http://stackoverflow.com/questions/17703553/bigrams-instead-of-single-words-in-termdocument-matrix-using-r-and-rweka)
seem to appear when processing larger data files. It still have to find out the why.










