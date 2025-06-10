---
title: "Water is Life - Are Native Americans right to be worried about DAPL?"
author: Nicco
date: 2016-12-21 10:15:03
layout: post
categories: [ Data Visualization, Tableau ]
image: assets/uploads/What's in DAPL way.webp
---

Mni Wiconi means "Water is life" in Lakota language, and it's a slogan Native Americans are using in their protests against DAPL: the Dakota Access Pipeline.

During the last year, protests against the pipeline's project became fierce, and the camp protesters set up has been referred as ["the largest Native American gathering since Little Bighorn"](http://www.bbc.co.uk/news/world-us-canada-37249617).

What concerns protesters, is that the pipeline is meant to run across protected area and sources of clean waters such as the Missouri river, the Mississipi river and the Lake Oahe, near the Standing Rock Indian reservation.

Mainly, the Standing Rock tribe is afraid that an oil spill could contaminate their drinking water. And they’re right to be. You don’t have to be an environmentalist to understand that oil and clean water don’t mix.

Let’s look at the numbers.

In the first dashboard, I collected data on U.S. pipeline incidents over the last 30 years. It’s public data, but scattered. I cleaned and pulled it into Tableau to make it easier to see trends. The picture is clear: pipeline spills are not rare. There have been over 11,000 significant incidents since 1986. That’s about one every day.

![workflow]({{ site.baseurl }}/assets/uploads/Are US pipelines SAFE.webp)

More than 5 million barrels of hazardous liquids have spilled. About half of those incidents involved crude oil. And almost a third of the time, the spill reached water.

This is what Standing Rock is worried about. They’re not imagining risks. They’re pointing to a pattern.

In the second dashboard, I mapped out what’s in DAPL’s path. The pipeline cuts through fragile zones: wetlands, rivers, and tribal lands. I overlaid protected areas and water bodies, then followed the line of the pipeline. It’s not a straight shot through empty land. It passes within half a mile of Lake Oahe, a key drinking water source for the Standing Rock Sioux.

![workflow]({{ site.baseurl }}/assets/uploads/What's in DAPL way.webp)

Both dashboards are built in Tableau. The first one uses incident data from PHMSA. I grouped incidents by year, cause, and material spilled. The second combines spatial data from the U.S. Geological Survey and the pipeline’s official route.

I didn’t build these to take a side. I built them to show what’s at stake: When people say “Mni Wiconi”, they’re not making a slogan. They’re stating a fact. Water is life. And pipelines leak.
