---
layout: post
title: Convex Hull - Grian Size Project
tags: [Linux, Java, OOP]
---

A few months ago I participated in a programming competition. The task was to calculate certain metrics from a group of objects (small rock grains) previously identified in an image. In this post, I will discuss the general idea of my approach with a demo example that could be used for different projects or used as a programming exercise. For this post I will use only one object, its sketch is showed in the following figure:

<img src="/img/non-convexhull.svg" alt="nonconvexhull" width="650" height="450" />

In a realistic scenario, one could read the points from a `CSV` file containing more than one object. Similarly to the use case described in [previous post](/blog/read-and-write-files-in-java/). For this example, I calculate the size of the object. For the given polygon we can simply compute the euclidean distance between each pair of points and take the maximum distance. This approach is not feasible because in some cases the number of points could be potentially large.

#### Implementation

For this implementation, I leverage the idea of `Convex Hull` to reduce the number of points. I choose a `Quick Hull` algorithm. It is based on a divide and conquer paradigm. The steps are defined as follows:

* On the `x` or `y` axis, find the minimum and maximum points (A, B respectively).
* Divide the set into two subsets by the line formed by connecting the points (A, B).
* On each subset, one at a time, find the point (C) with the maximum distance from the line.
* By connecting the points (A, C) and (B, C) we will have two new lines. Repeat the previous two steps recursively until no points left.

The `java` implementation of this algorithm can be found at my [GitHub repository](https://github.com/baldmer/quickhull).

Next, I define the class that uses the `QuickHull` algorithm and implements the logic to alleviate the problem. The class is defined as follows:

{% highlight java linenos %}

import java.util.ArrayList;

public class GrainSize
{
    public static void main(String args[])
    {
        // main body
    }
}
 
{% endhighlight %}
 
In the body of the main method, I first process the string of points and create an `ArrayList`.

{% highlight java linenos %}

String line = "3150,1486,3155,1486,3157,1489,3159,1492,3162,1494,3164,"
            + "1497,3168,1498,3170,1501,3172,1504,3173,1508,3169,1509,3165,"
            + "1508,3161,1507,3158,1507,3154,1506,3153,1502,3151,1499,3151,"
            + "1494,3149,1491,3149,1488";

ArrayList<Point> points = new ArrayList<Point>();
String[] values = line.split(",");

for (int i = 0; i < values.length; i += 2){
    int x = Integer.parseInt(values[i]);
    int y = Integer.parseInt(values[i + 1]);
                     
    Point p = new Point(x, y);
    points.add(p);
}
		
{% endhighlight %}
		
Then I compute the `Convex Hull` of the set of points. I create an instance of the `QuickHull` class and run the algorithm as follows:

{% highlight java linenos %}

QuickHull quickHull = new QuickHull();
ArrayList<Point> convexHull = quickHull.run(points);

{% endhighlight %}

The resulting `Convex Hull` polygon will look like in the following figure:


<img src="/img/convexhull.svg" alt="convexhull" width="650" height="450"/>


We can observe that only the outer points have been kept, which will reduce substantially the number of distance calculations between points.

Finally, I compute the euclidean distance as discussed early and get the diameter of the object.

{% highlight java linenos %}

double diameter = 0.0;
int convexHullSize = convexHull.size();
		
for (int i = 0; i < convexHullSize; i ++){
    for (int j = 0; j < convexHullSize; j ++){
                        
        Point p1 = convexHull.get(i);
        Point p2 = convexHull.get(j);
                        
        double distance = p1.euclideanDistance(p2);
                        
        if (distance > diameter)
            diameter = distance;
    }
}

{% endhighlight %}
 




