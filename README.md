# GAP Climate Research

This repository holds the analysis for quantifying avoided carbon impacts from the virtualization of the American Psychiatric Association's (APA) 2020 Annual Meeting. 

The APA is the largest psychiatric organization in the world at almost 40 thousand members. Each year approximately 15 thousand of these members attend the Annual APA Meeting held at locations across the United States.

In the wake of the Coronavirus pandemic the APA canceled the 2020 annual meeting at the Pennsylvania Convention Center scheduled for April 25-29 and announced a virtual substitute, the *2020 APA On Demand online CME Product.*

This research is currently in progress and supported by a team of psychiatrists at the think tank [Group for the Advancement of Psychiatry (GAP)](https://ourgap.org) and myself.

Below are some visualizations of our flight-network model for APA Annual Meeting attendees. Each point represents a single attendee's origin. The curved lines represent the geodesic distance or the path *as the crow flies* that each attendee will take to arrive at the meeting's location. This pathway is the shortest arc between two points on the great circle, or Earth's surface. The width and intensity of the arc represents the relative frequency that pathway is travelled.

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

In-person conferences are valuable opportunities for like-minded professionals to network and share their research. Yet, the avoided carbon footprint virtualizing such large meetings provides may not always outweigh the value of an in-person experience.

Our research leads us naturally to question the extent to which we can mitigate the carbon footprint of such large meetings if and when they are held in-person. The [International Civil Aviation Organization (ICAO)](https://www.icao.int/), an agency of the United Nations, has even developed a *Green Meetings Calculator* to promote awareness among conference organizers of CO<sub>2</sub> emission costs from air travel.

We can leverage the historical attendance data from each conference to help locate regions that reduce the carbon footprint of the commute. We use the data visualized above to reduce the total distance attendees will travel, since travel (especially air) is by far the largest contributor to an individual's carbon footprint.

I have designed an algorithm to approximate the *geometric median*, or *centroid*, of the APA's attendance base over the past 3 years. The geometric median is the coordinate which minimizes the sum of distances attendees travel. Traditionally, this problem has been the subject of branches within computational geometry and operations research often under the labels *facility location* or the *Weber problem.*

While theoretical closed from solutions for the geometric median exist in special cases, it is more practical to arrive at trivially-approximate solutions using iterative approaches. Moreover, our context is somewhat more complex than traditional facility location since we must work in a non-Euclidean space to account for the non-spherical curvature of the Earth.

In the Euclidean context, the geometric median is defined by the *L<sup>1</sup>* norm, but has more recently been generalized to Riemannian manifolds as the *Riemannian median*, both of which lack a closed form solution (see [Yang (2009)](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwirhdPqtrPqAhWSOn0KHRCCCvkQFjABegQIARAB&url=https%3A%2F%2Farxiv.org%2Fabs%2F0911.3474&usg=AOvVaw2Vb8S3kgGuxSr7QtcKzFS9) and [Drezner and Wesolowsky (1978)](https://www.jstor.org/stable/3009474?seq=1)).

We can think of a norm as a function that maps a vector to the positive real-number line [0, *+inf*]. Given a vector *x* with *i* components, we can define a general *p-norm* as ***||x||<sub>p</sub>*** :

<img style="float: right; text-align: center;" src="https://render.githubusercontent.com/render/math?math=||x||_p = \left( \sum_i|x_i|^p \right)^{\frac{1}{p}}">

Thus, we can define the geometric median of this *i-space* when we set *p = 1.*

## Optimal APA Locations

Below is a map of optimal meeting regions approximated by the algorithm. We see the regions differ slightly: 

![APA Facility Location](/images/Facility-Location.jpg)

This difference is due to variation in the attedance base across years. Because conference attendees are more likely to attend conferences that are closer, bias is introduced when we only look at a single year's data. 

We see this geographic incentive in the centroid (denoted by the cross) of the above polygons. Each year's optimal region is pulled toward the conference center.

Thus, in order form a robust understanding it is important to have geographically diverse conference data.