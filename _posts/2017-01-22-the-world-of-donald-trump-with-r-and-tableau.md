---
title: "The world of Donald Trump, visualized with R and Tableau"
author: Nicco
date: 2017-01-22 16:50:52
layout: post
categories: [ Data Visualization, Tableau, Data Engineering, R ]
image: assets/uploads/trump-world.webp
---

Last week, Buzzfeed released the output of a research they have carried on [all the connections that Donald Trump holds with people and organizations](https://www.buzzfeed.com/johntemplon/help-us-map-trumpworld?utm_term=.id9yRvkqnp#.diLxNgZGkw).

I decided I wanted to visualize this dataset on a network chart, showing both the direct and indirect business connections of Trump. See below the end result (click for the interactive version).

[caption id="attachment\_1430" align="alignnone" width="997"][![trump-world]({{ site.baseurl }}/assets/uploads/trump-world.webp)](https://public.tableau.com/views/TrumpWorld/Dashboard1?:embed=y&:display_count=yes) [Trump's world: A network of Trump's connections](https://public.tableau.com/views/TrumpWorld/Dashboard1?:embed=y&:display_count=yes)[/caption]



In order to plot the chart, we need a model that assign sensible coordinates to each of the points, and unfortunately this is not natively available in Tableau.

However, we can find a number of such models in R! And for free!

All we need is some simple data preparation, seven lines of R code, a bit of very easy data wrangling, and we are done.

Having an alteryx license, I used that software to perform all those steps in a single workflow. If you do have alteryx, you can download the workflow [here](https://www.dropbox.com/s/q15qnq9fqxlvy70/Trump%20network%20diagram.yxmd?dl=0).

If you don't have alteryx, don't worry, I am going to tell you how to replicate all the steps using only excel and R studio.

First, let's have a look at how the alteryx workflow works so we can identify the actions we'll need to take in order to get the network chart done.

![annotated-workflow]({{ site.baseurl }}/assets/uploads/annotated-workflow.webp)

Now, let's see how to replicate each of those steps:
1. Download the google spreadsheet with the data [here](https://docs.google.com/spreadsheets/d/1Z5Vo5pbvxKJ5XpfALZXvCzW26Cl4we3OaN73K9Ae5Ss/edit);
2. Create a column in each sheet of the spreadsheet with the Link Type ("Org-Org","Person-Org","Person-Person");
3. In each sheet, rename all the columns signed with "A", as just "A"(e.g. "Organization A" -> "A") and all the columns signed with "B" as just "B";
4. Now unify the three spreadsheets into a single one, copying and pasting them to a new sheet. Make sure to match each column ("A" with "A", "B" with "B", etc), and to just have the headers once, at the beginning;
5. Save the resulting dataset as a csv file. It should look like this:


[caption id="attachment\_1471" align="alignnone" width="616"]![dataset-for-r]({{ site.baseurl }}/assets/uploads/dataset-for-r.webp) We cleaned the data, and are ready to go to R![/caption]

In R studio:
1. Import the dataset. You can use Tools>Import dataset, or just write in your console:

> > Trump.dataset <- read.csv("[Your file path]/Trump dataset.csv")> View(Trump.dataset)


2. Install "igraph" package. You can use Tools>Install packages, or just write:

> > install.packages("igraph")


3. Declare the package, and build the model. Write in your console:

> > install.packages("igraph")
> 
> > library(igraph)



> > g <- graph.data.frame(Trump.dataset, directed=TRUE)



> > plot(g, layout=layout.fruchterman.reingold)#fruchterman.reingold(g)*0.2)#, layout=l*1.0)


4. Export your results as a new csv file. Write:

> > write.csv(data.frame(layout.fruchterman.reingold(g),vertex\_attr(g)),file = "[Your file path]/Trump network in R.csv")




Note that "fruchterman.reingold" is the model I chose for the network chart, but you can pick any of the available (see [here](http://igraph.org/c/doc/igraph-Layout.html)), and get different results. Equally, you can change the other parameters.

Now you have a csv with the coordinates and names of each point of the network chart. Something like this:

![r-result]({{ site.baseurl }}/assets/uploads/r-result.webp)

We are close now. In order to get the desired result in Tableau, we need to go back to excel and just perform a few more steps.
1. We need to create a new excel spreadsheet where we copy the first csv file, the one we inputted in R, adding two columns, both called "path", one with "1" for every record, and one with "2".
2. We also need a new column called "Link", where we combine "A" and "B", with a formula like:

> Link =A2&" <-> "&B2


3. We then pivot the "A" and "B" columns to a single column called "name". I would just copy all the columns except from "A" and the "path" with "2", then paste them after the last record of data (making sure column match - like "A" match with "B", etc.). I would then delete the "B" column and the "path" column with 2 from the first half of the data, shifting cells left.


The result should look like this:

![pivot-result]({{ site.baseurl }}/assets/uploads/pivot-result.webp)

Now the last bit is to setup a lookup that assign for each "Name" the coordinates we got from R.
1. Copy the csv file resulting from R in the spreadsheet where you are working;
2. Switch the order of the columns so to have "Name" | "X1" | "X2"
3. In the main sheet, create two new columns, "X" and "Y", and setup a lookup formula for each that gets the coordinates from the other sheet. Like the following:

> X =VLOOKUP('Trump dataset'!A2,'Trump network in R 2 - Copy'!$A$2:$C$1515,2,0)



> Y =VLOOKUP('Trump dataset'!A2,'Trump network in R 2 - Copy'!$A$2:$C$1515,3,0)


4. The result should look like this:![Final dataset.PNG]({{ site.baseurl }}/assets/uploads/final-dataset.webp)


Now we have our dataset ready for Tableau!

In Tableau, just bring Y as columns and X as rows (both as continuous dimension), select "line" and drag "link" on detail and "path" on path.

Dual-axis the "X" (or the "Y"), select "dots" and drag "name" into detail.

Synchronize the axis, and you are done.

Feel free to ask for any additional explanation in the comments or on twitter, and share with me your network charts!

![canvas.PNG]({{ site.baseurl }}/assets/uploads/canvas.webp)
