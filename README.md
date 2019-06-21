##  __I am busy breaking, fixing, testing, and refining (June 17th--23rd)__

>  (  This will become available for use in concert with its announcement on Discourse.  )
>
> _please know that your interest and attentiveness are matters of moment and import_    

----
----

# FastRationals.jl

### rationals with unreal performance <sup>[𝓪](#source)</sup>

##### Copyright © 2017-2019 by Jeffrey Sarnoff. This work is released under The MIT License.
----

### computing with rational arithmetic

|                         |   |
|:------------------------|:----------------:|
|                         |   relative speed               |
| FastRational{ Int32 }   |    6 .. 12     |
|                         |                  |
| SystemRational{ Int32 } |        1        |

----

### Benchmarking

```
using FastRationals, Polynomials, BenchmarkTools

w,x,y,z = Rational{Int32}.([1//12, -2//77, 3//54, -4//17]); q = Rational{Int32}(1//7);
a,b,c,d = FastRational.((w,x,y,z)); p = FastRational(q);

poly = Poly([w,x,y,z])
fastpoly = Poly([a,b,c,d])

polyval(poly, q) == polyval(fastpoly, p)
# true

relative_speedup =
    floor((@belapsed polyval($poly, $q)) / (@belapsed polyval($fastpoly, $p)))

# relative_speedup is ~16
```

```
using FastRationals, BenchmarkTools

x, y, z = Rational{Int32}.((1234//345, 345//789, 987//53))
a, b, c = FastRational.([x, y, z])

function test(x,y,z)
   a = x + y
   b = x * y
   c = z - b
   d = a / c
   return d
end

test(x,y,z) == test(a,b,c)
# true

relative_speedup =
    floor( (@belapsed test(Ref($x)[],Ref($y)[],Ref($z)[])) / 
           (@belapsed test(Ref($a)[],Ref($b)[],Ref($c)[])))

# relative_speedup is ~4
```

Arithmetic works like `Rational` for eltypes `Int8, .., Int128, UInt8, ..` except there is no Infinity, no NaN comparisons.

----

<sup><a name="source">[𝓪](#attribution)</a></sup> Harmen Stoppels on 2019-06-14
