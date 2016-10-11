---
layout: post
title:  "Using Chart.js in Your React App"
date:   2016-09-25 09:00:00
categories: code, javascript, react
---

Chart.js is a simple, easy-to-use tool for adding data visualization to your app. In this blog post, I’ll give you a quick walkthrough of how to use this tool in React, and show a couple of examples of charts.

I’ll be using <code>react-chartjs-2</code>(https://github.com/reactjs/react-chartjs) to create a React components for each chart.

(Note that <code>react-chartjs</code> is the original library — however it is reliant on an earlier version of chart-js than the one I will be using in this demonstration).

Installation
------------

First, install the two required libraries:

{% highlight javascript %}
npm install --save react-chartjs-2 chartjs
{% endhighlight %}

You will also need Reach and React-dom, but if you're building a React app, I assume you already have those installed.


Setting Up Your First Chart
---------------------------

We're going to start with a simple pie chart. For this example, we're going to show the distribution of chores in a household.

First, we will import the Pie component from <code>react-chartjs-2</code>.

{% highlight javascript %}
import {Pie} from 'react-chartjs-2';
{% endhighlight %}

Then, we'll create a stateless component for our chart, and export it:

{% highlight javascript %}
const ChoresChart = (props) => {
  const data = {}
  const options = {}

  return (
    <Pie data={data} options={options} />
  )
}

export default ChoresChart;
{% endhighlight %}

Note that I've added two constants (currently as empty objects, but we'll add to them shortly), <code>data</code> and <code>options</code>, and passed them as props to the ChoresChart component. I'll go into the details of these props next.

Data
----

To display data, the chart must be passed a data object that contains all of the information needed by the chart. The data object must contain the dataset(s) that will populate the chart. In addition, there are other attributes that may be passed in to customize the chart (for example, labels for your data).

{% highlight javascript %}
const data = {
  labels: ["Gentian", "Alice", "Alex"],
  datasets: [
    {
      data: [2, 5, 11],
    backgroundColor: [
      "#FF6384",
      "#36A2EB",
      "#FFCE56"
      ]
    }
  ]
}
{% endhighlight %}

I've added a labels array for the data in my set. The index of the labels array is matched to the corresponding index of the data array within the dataset. So, here we can surmise that Gentian has been assigned 2 chores, Alice has been assigned 5, and Alex a rather unfair 11 (sorry, Alex).

In addition, I've added a background color for each data point, again matched by array index.

Options
-------
There are multiple options you can add to your chart. I'm going to add a relative simple one -- a title.

{% highlight javascript %}
const options = {
  title: {
    display: true,
    text: "Chore Distribution for this Month",
    fontFamily: "Roboto",
    fontSize: 20,
  }
}
{% endhighlight %}

This is relatively self-explanatory -- I'm setting the display to true (by default a chart will not have a title), and adding the title text and a little styling.

The Result
----------

<img src="/images/chorepiechart.png" caption="a beautiful pie chart" />

One more example
----------------

Here is a full example of a bar chart, representing two years of bills:

{% highlight javascript %}
import React from 'react';
import {Bar} from 'react-chartjs-2';

const BillsChart = (props) => {
  const data = {
    labels: ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],
    datasets: [
      {
        data: [150, 125, 300, 250, 300, 95, 150, 75, 200, 200, 199, 75],
        backgroundColor: "#FF6384",
        label: 2015
    },
      {
        data: [175, 140, 275, 300, 300, 100, 175, 90, 225, 200, 225, 100],
        backgroundColor: "#FF7",
        label: 2016
    }
    ]
  }

  const options = {
        scales: {
            yAxes: [
              {
                  ticks: {
                     callback: function(label, index, labels) {
                       return '$' + label;
                     }
                  }
              }
            ]
        },

    }

  return (
    <Bar data={data} options={options} />
  )
}

export default BillsChart;
{% endhighlight %}

This will produce the following chart:

<img src="/images/billsbarchart.png" />

The data is similar to the pie chart, except in this case we have two datasets, one for each of the two years we are comparing. I've added a label attribute to each dataset to identify the year.

The options are more interesting here. Let's take a closer look:

{% highlight javascript %}
const options = {
      scales: {
          yAxes: [
            {
                ticks: {
                   callback: function(label, index, labels) {
                     return '$' + label;
                   }
                }
            }
          ]
      },
  }
{% endhighlight %}

The issue with the plain data is that it does not identify the numbers provided as dollar amounts. In order to have the tick mark label on the y-axis prefixed with '$' (as in the chart above) we need to create a nested object, and add a callback function to the <code>ticks</code> attribute. This function takes in the ticks label, its index, and the array of ticks labels, and for each label, returns the label prefixed with '$'.

Resources
---------

* [Chart.js Documentation](http://www.chartjs.org/)
* [chart-js-2 on Github](https://github.com/gor181/react-chartjs-2)
