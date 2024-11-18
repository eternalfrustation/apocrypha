# 2D Rendering

- Denote an image with a single array and a pixel's index can be found by `width * y + x`.
- The x coordinate is `i % width`, y is `i / width`.
- To get a point at a distance r and angle t, relative to point (x0, y0)
$$ x = x0 + r * cos(t) $$
$$ y = y0 + r * sin(t) $$
- For drawing a simple primitive, use its derivative to get the next pixel, usually the derivative is simpler
- Ok, Fk, This stuff is sub optimal, since it kinda encourages drawing vertically, which is not optimal for the cache.

## Line

- A line's equation is 

$$ y - y0 = m * (x - x0) $$ or,
$$ x - x0 = (y - y0) / m $$
where $m = (y1 - y0) / (x1 - x0)$

- When drawing line, a simple `for (x in x0..x1)` usually leaves gaps if $m > 1$, and `for (y in y0..y1)` leaves gaps when $m < 1$, use both with if cases.
- Use $y' = m$ when $x = 1$ when drawing lines to avoid division.

## Circle

- A circle's equation is

$$ (x - x0)^2 + (y - y0)^2 = r^2 $$
or,
$$ y - y0 = \sqrt(r^2 - (x - x0)^2)$$

- Its not worth doing the derivative for a circle, just use the equation.
- A circle can be drawn by only finding 1/8th of it, and mirroring it to the other 7 parts.

## Ellipse

- An ellipse's equation is

$$ (x - x0)^2 / a^2 + (y - y0)^2 / b^2 = 1 $$
or, 
$$ y - y0 = b * sqrt(1 - (x - x0)^2 / a^2) $$

- Its not worth doing the derivative for a circle, just use the equation.
- An ellipse can be drawn by only finding 1/4th of it, and mirroring it to the other 3 parts.

## Rotated Rectangle

- $angle = angle (\bmod \pi/2)$
- Find the points of upper line and the corresponding point at the bottom part at the same x coordinate.
- Draw a vertical line between these two points.
- Bam, you have a rotated rectangle.
