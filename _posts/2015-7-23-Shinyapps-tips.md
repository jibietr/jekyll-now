---
layout: post
title: Shinyapps.io tips
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

see a related post (here)[http://stackoverflow.com/questions/26316192/uploading-csv-file-to-shinyapps-io]


**tip #2: `showLogs` is your friend**

this is quite basic, but I only got to knew about it when I 
started getting into complex deployment errors. Use it after deploying
to monitor the deployment result:

```R
shinyapps::showLogs(appName='my_app_name')
```

**tip #3: `require` all the libraries you need**

I run into issues with libraries that were not available in shinyapps.io
at running time, most likely to broken dependencies of some packages.
A way to avoid this is to explicitly load all the libraries
that you are using in your local session. You use `showInfo()`
to check what are the libraries under `other attached packages` and
use `require` to explicitly load them in your `.Rmd` file.


**tip #4: Use options(mc.cores=1) when using `tm` and `parallel` package**

Again, I found several different errors when using functions from 
the `tm` package that depend on `parallel`. Using `options(mc.cores=1)` solves
the problem in one shot. I experienced this with larger data files...

An alternative to this is to use lazy=TRUE
[here](http://stackoverflow.com/questions/25069798/r-tm-in-mclapplycontentx-fun-all-scheduled-cores-encountered-errors)


This goes on the line of what is discussed here: [here][http://stackoverflow.com/questions/26834576/big-text-corpus-breaks-tm-map),
[here](http://stackoverflow.com/questions/17703553/bigrams-instead-of-single-words-in-termdocument-matrix-using-r-and-rweka)






