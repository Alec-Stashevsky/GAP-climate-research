# GAP Climate Research

This repository holds the analysis for quantifying avoided carbon impacts from the virtualization of the American Psychiatric Association's (APA) 2020 Annual Meeting. 

The APA is the largest psychiatric organization in the world at almost 40 thousand members. Each year approximately 15 thousand of these members attend the Annual APA Meeting held at locations across the United States.

In the wake of the Coronavirus pandemic the APA canceled the 2020 Annual Meeting at the Pennsylvania Convention Center scheduled for April 25-29 and announced a virtual substitute, the *2020 APA On Demand online CME Product.*

This research is currently in progress and supported by a team of psychiatrists at the think tank [Group for the Advancement of Psychiatry (GAP)](https://ourgap.org) and myself.

Here are some teaser visualizations of our flight-network model for APA Annual Meeting attendees:

## Javitz Center 2018 APA Annual Meeting

![NYC 2018 APA Annaul Meeting](/images/NYC2018.jpg)

<br />
<br />

## Moscone Center 2019 APA Annual Meeting

![SF 2019 APA Annaul Meeting](/images/SF2019.jpg)

<br />
<br />

## Pensylvania Convention Center 2020 APA Annual Meeting 

### This is a Counterfactural Scenario

![PHL 2020 APA Annaul Meeting](/images/PHL2020.jpg)

<br />
<br />

## Optimal Meeting Location Algorithm

As part of our research, I have designed an algorithm to approximate the *geometric median*, or *centroid*, of the APA's historical attendance base each year. Traditionally, this problem has been the subject of branches within computational geometry and operations research often under the labels *facility location* or the *Weber problem.*

While theoretical closed from solutions for the geometric median exist in special cases, it has been more practical to arrive at trivially-approximate solutions using iterative approaches. Moreover, our context is somewhat more complex than traditonal facility location since we must work in a non-Euclidean space to account for the ellipsoidal curvature of the Earth.

In the non-Euclidean context, the geometric median is defined by the *L<sup>1</sup>* norm and has more recently been generalized to Reimannian manifolds as the *Riemannian median*, both of which lack a closed form solution (see [Yang (2009)](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwirhdPqtrPqAhWSOn0KHRCCCvkQFjABegQIARAB&url=https%3A%2F%2Farxiv.org%2Fabs%2F0911.3474&usg=AOvVaw2Vb8S3kgGuxSr7QtcKzFS9), [Drezner and Wesolowsky (1978)](https://www.jstor.org/stable/3009474?seq=1))

We can think of a norm as a function that maps a vector to the positive real-number line **[0, *+inf*]**. Thus, we can define a general "*p-norm*" as **_||x||<sub>p</sub>**_ given a vector *x* with *i* components:

<img style="float: right;" src="https://render.githubusercontent.com/render/math?math=||x||_p = \left( \sum_i|x_i|^p \right)^{\frac{1}{p}}">

Thus, we can define the geometric median of this *i-space* when we set *p = 1.*

## Optimal APA Locations

Below is a map of optimal meeting regions approximated by the algorithm. Since each year has a slightly different attendance base, they do not overlap. Since we only have historical attendance data available, it is important to consider the confouding incentives created by the meeting's geography. Attendees who are closer to the meeting location that year are more likely to attend the meeting, and we see this reflected in the geometric median of each year, denoted by the golden cross within each polygon.

![APA Facility Location](/images/Facility-Location.jpg)