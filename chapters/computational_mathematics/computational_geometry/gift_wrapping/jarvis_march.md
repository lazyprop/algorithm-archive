<script>
MathJax.Hub.Queue(["Typeset",MathJax.Hub]);
</script>
$$ 
\newcommand{\d}{\mathrm{d}}
\newcommand{\bff}{\boldsymbol{f}}
\newcommand{\bfg}{\boldsymbol{g}}
\newcommand{\bfp}{\boldsymbol{p}}
\newcommand{\bfq}{\boldsymbol{q}}
\newcommand{\bfx}{\boldsymbol{x}}
\newcommand{\bfu}{\boldsymbol{u}}
\newcommand{\bfv}{\boldsymbol{v}}
\newcommand{\bfA}{\boldsymbol{A}}
\newcommand{\bfB}{\boldsymbol{B}}
\newcommand{\bfC}{\boldsymbol{C}}
\newcommand{\bfM}{\boldsymbol{M}}
\newcommand{\bfJ}{\boldsymbol{J}}
\newcommand{\bfR}{\boldsymbol{R}}
\newcommand{\bfT}{\boldsymbol{T}}
\newcommand{\bfomega}{\boldsymbol{\omega}}
\newcommand{\bftau}{\boldsymbol{\tau}}
$$

# Jarvis March

The first two-dimensional convex hull algorithm was originally developed by R. A. Jarvis in 1973 {{ "jm1973" | cite }}.
Though other convex hull algorithms exist, this algorithm is often called *the* gift-wrapping algorithm.

The idea behind this algorithm is simple.
If we start with a random distribution of points, we can find the convex hull by first starting with the left-most point and using the origin to calculate an angle between every other point in the simulation.
Whichever point has the largest interior angle is chosen as the next point in the convex hull and we draw a line between the two points.
From there, we use the two known points to again calculate the angle between all other points in the simulation.
We then choose the point with the largest interior angle and move the simulation forward.
We keep repeating this process until we have returned to our priginal point.
The set of points chosen in this simulation will be the convex hull. 
Here is what this might look like in code:

```julia
type Pos
    Float64 x,y
end

function jarvis_march(Vector{Pos} points)
    Vector{Pos} hull
    hull.append(leftmost(points))

    Int64 i = 0
    Pos curr_point = points[0]
    Float64 curr_theta = angle({0,0}, hull[0], curr_point)
    while (curr_point != hull[0])
        for point in points
            Float64 theta
            if (i > 0)
                theta = angle(hull[i-1], hull[i], point)
            else
                theta = angle({0,0}, hull[i], point)
            end

            if (theta > curr_theta)
                curr_point = point
                curr_theta = theta
            end
        end
        hull.append(curr_point)
        i++
    end

    return hull
end
```

As we might expect, this algorithm is not incredibly efficient and has a runtime of $$\mathcal{O}(nh)$$, where $$n$$ is the number of points and $$h$$ is the size of the hull.
As a note, the Jarvis March can be generalized to higher dimensions.
Since this algorithm, there have been many other algorithms that have advanced the field of two-dimensional gift-wrapping forward, including the Graham Scan and Chan's Algorithm, which will be discussed in due time.

### Bibliography

{% references %} {% endreferences %}

### Example Code

To be added later!