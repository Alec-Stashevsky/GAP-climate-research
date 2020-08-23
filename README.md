# GAP Climate Research Overview

This repository holds the analysis for quantifying avoided carbon impacts from the virtualization of the American Psychiatric Association's (APA) 2020 Annual Meeting. 

The APA is the largest psychiatric organization in the world at almost 40 thousand members. Each year approximately 15 thousand of these members attend the Annual APA Meeting held at locations across the United States.

In the wake of the Coronavirus pandemic the APA canceled the 2020 annual meeting at the Pennsylvania Convention Center scheduled for April 25-29 and announced a virtual substitute, the *2020 APA On Demand online CME Product.*

This research is currently in progress and supported by a team of psychiatrists at the think tank [Group for the Advancement of Psychiatry (GAP)](https://ourgap.org) and myself.

Below are some visualizations of our flight-network model for APA Annual Meeting attendees. Each point represents a single attendee's origin. The curved lines represent the geodesic distance or the path *as the crow flies* that each attendee will take to arrive at the meeting's location. This pathway is the shortest arc between two points on the great circle, or Earth's surface. The width and intensity of the arc represents the relative frequency that pathway is traveled.

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

This difference is due to variation in the attendance base across years. Because conference attendees are more likely to attend conferences that are closer, bias is introduced when we only look at a single year's data. 

We see this geographic incentive in the centroid (denoted by the cross) of the above polygons. Each year's optimal region is pulled toward the conference center.

Thus, in order to form a robust understanding it is important to have geographically diverse conference data.

# Methodolgy

To calculate emissions from the raw APA Annual Meeting attendance data, a variety of open data and tools have been leveraged. Below is a diagram illustrating how the raw data is geo-coded and subsequently used to estimate travel emissions for both air and ground commutes. 

![APA Methodology Diagram](/images/APA-Flow-Diagram.jpg)

Two API's were vital to the analysis. Using the Google Maps API via the _{ggmap}_ package, we could easily geo-code imperfect location data provided by APA attendees. We then acquired a dataset of commercial airports across the globe from [partow.net](https://www.partow.net/miscellaneous/airportdatabase/) and used this to locate airports attendees would likely depart from, provided they are outside a reasonable driving distance to the meeting of 400 km. 

Secondly, we are grateful to GoClimate for lending us access to their commercial flight emissions API. After an attendee's likely departure and arrival airports were determined, we leveraged GoClimate's robust [flight emissions methodolgy](https://www.goclimate.com/blog/wp-content/uploads/2019/04/Calculations-in-GoClimateNeutral-Flight-Footprint-API.pdf) to determine the round-trip carbon emissions per passenger.

To determine carbon emissions due to ground transportation, we assumed the standard [EPA guidlines for passenger vehicles (March 2018).](https://nepis.epa.gov/Exe/ZyNET.exe/P100U8YT.TXT?ZyActionD=ZyDocument&Client=EPA&Index=2016+Thru+2020&Docs=&Query=&Time=&EndTime=&SearchMethod=1&TocRestrict=n&Toc=&TocEntry=&QField=&QFieldYear=&QFieldMonth=&QFieldDay=&IntQFieldOp=0&ExtQFieldOp=0&XmlQuery=&File=D%3A%5Czyfiles%5CIndex%20Data%5C16thru20%5CTxt%5C00000007%5CP100U8YT.txt&User=ANONYMOUS&Password=anonymous&SortMethod=h%7C-&MaximumDocuments=1&FuzzyDegree=0&ImageQuality=r75g8/r75g8/x150y150g16/i425&Display=hpfr&DefSeekPage=x&SearchBack=ZyActionL&Back=ZyActionS&BackDesc=Results%20page&MaximumPages=1&ZyEntry=1&SeekPage=x&ZyPURL) 

Emissions from ground transportation arise from two distinct segments of an attendee's potential commute.
If an attendee is less than 400 km away from a conference destination, the the geodesic distance between the attendee's home and the conference destination is used as a proxy for the total distance driven to the conference and back. If an attendee is greater than 400 km away from a conference destination, the geodesic distance between the attendee's home and nearest commercial airport is used as a proxy for the distance driven to the nearest airport and back.

It is important to note that the geodesic distance will always fall short of the total driving distance, and thus we are underestimating the total emissions which arise from driving. This is to maintain consistency with our outlook which aims for a conservative estimate of the meeting's carbon footprint. 

Moreover, because we cannot pinpoint an attendee's origin location beyond the city-level, the true distance attendees will have to travel from their homes to the closest airport will vary considerably. Due to this lack of resolution we have omitted driving emissions which may arise from any ground transport taking place between the arrival airport and the conference center. Large conference centers are generally located close to international airports thus it is likely this leg of the commute plays a less influential role in the meeting's overall carbon footprint, the majority of which we know arises from air travel. 