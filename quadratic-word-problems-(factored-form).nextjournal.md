# Quadratic word problems (factored form)

Amir stands on a balcony and throws a ball to his dog, who is at ground level.

The ball's height (in meters above the ground), $x$ seconds after Amir threw it, is modeled by

$h(x)=-(x+1)(x-7)$

```python id=5c1e52c0-4b86-4890-b1e3-7dbb9f2b6954
from sympy import *

x = Symbol('x')

coeff = -1
term1 = 1
term2 = -7
y = coeff * (x + term1)*(x + term2)
zero1 = solveset(Eq(x + term1))
zero2 = solveset(Eq(x + term2))
vertx = sum(zero1 + zero2) / 2
verty = y.subs(x, vertx)
vertex = (vertx, verty);
graph = {'Vertex': vertex, 'Zeros': zero1 + zero2}
graph
```

[text/plain](https://nextjournal.com/data/QmSkeRSqstGBqHLWkpJQ27hLnStdWyPLggb8BNxwMJvGde?content-type=text%2Fplain&filename=text%2Fplain)

**What is the maximum height that the ball will reach?**

```python id=587e03c2-ec9c-40cf-bb98-a35a30d539fa
verty
```

[text/plain](https://nextjournal.com/data/QmaHXADi79HCmmGPYMmdqvyemChRmZPVGyEQYmo6oS2C3a?content-type=text%2Fplain&filename=text%2Fplain)

**How many seconds after being thrown will the ball reach its maximum height?**

```python id=d8366b3f-5fea-45c0-9ce5-d549b6529b02
vertx
```

[text/plain](https://nextjournal.com/data/QmTbEvXabndtyFm6zYZBvYg82jy9Po9bX8dXbyPAEWYh1f?content-type=text%2Fplain&filename=text%2Fplain)

The ball's height is modeled by a quadratic function, whose graph is a parabola.

The maximum height is reached at the vertex.

So in order to find the maximum height, we need to find the vertex's $y$\-coordinate.

We will start by finding the vertex's $x$\-coordinate, and then plug that into $h(x)$.

The vertex's $x$\-coordinate is the average of the two zeros, so let's find those first:

$\\begin{aligned} h(x)&=0 \\\\ -(x+1)(x-7)&=0 \\\\ \\swarrow\\quad&\\searrow \\\\ x+1=0\\text{ or }&x-7=0 \\\\ x={-1}\\text{ or }&x={7} \\end{aligned}$

Now let's take the zeros' average:

$\\dfrac{({-1})+({7})}{2}=\\dfrac62={3}$

The vertex's $x$\-coordinate is $3$. Now let's find $h({3})$:

$\\begin{aligned} h( 3)&=-( 3+1)( 3-7) \\\\ &=-(4)(-4) \\\\ &=16 \\end{aligned}$

**In conclusion, the maximum height that the ball will reach is $16$ meters.**

Ana dives into a pool off of a springboard high dive.

Her height (in meters above the water), $x$ seconds after diving, is modeled by

$$
h(x)=-5(x+1)(x-3)
$$

**What is the height of Ana above the water at the time of diving?**

```python id=2b075bb7-349d-4ae2-9c63-a71e1ec386d6
-5*(0 + 1)*(0 - 3)
```

[text/plain](https://nextjournal.com/data/Qmdq7c5kpLfsWi4aQpDM2T7g6pUz48iSdmKK6XyfZ9HkUr?content-type=text%2Fplain&filename=text%2Fplain)

**How many seconds after diving will Ana hit the water?**

Ana hits the water when $h(x)=0$.

$\\begin{aligned} h(x)&=0 \\\\ -5(x+1)(x-3)&=0 \\\\ \\swarrow\\quad&\\searrow \\\\ x+1=0\\text{ or }&x-3=0 \\\\ x=-1\\text{ or }&x=3 \\end{aligned}$

We found that $h(x)=0$ for $x=-1$ or $x=3$. Since $x=-1$ doesn't make sense in our context, the only reasonable answer is $x=3$.

In conclusion, Ana hit the water after $3$ seconds.

Simon has $160$ meters of fencing to build a rectangular garden.

The garden's area (in square meters) as a function of the garden's width $w$ (in meters) is modeled by

$A(w)=-w(w-80)$

**What width will produce the maximum garden area?**

The maximum area is reached at the vertex. So in order to find the width that produces the maximum area, we need to find the vertex's $x$\-coordinate.

```clojure id=3ab0d5e5-b2b7-419b-82f1-b20053934e7e
(defn root [[b c]]
  (- (/ c b)))

(defn roots [eq1 eq2]
  [(root eq1) (root eq2)])

(defn vertex [n f1 f2]
  (let [x (/ (+ (root f1) (root f2)) 2)
        y (* n
             (+ (* (first f1) x)
                (last f1))
             (+ (* (first f2) x)
                (last f2)))]
    [x y]))

{:vertex (vertex 1 [-1 0] [1 -80])
 :roots (roots [-1 0] [1 -80])}
```

In conclusion, the maximum area is produced when the rectangle's width is $40$ meters.

**What is the maximum area possible?**

In order to find the maximum area, we need to find the vertex's $y$\-coordinate.

Guillermo is a professional deep water free diver.

His altitude (in meters relative to sea level), $x$ seconds after diving, is modeled by

$g(x)=\\dfrac{1}{20}x(x-100)$

**What is the lowest altitude Guillermo will reach?**

To find the lowest altitude, we need to find the vertex's $y$\-coordinate.

```clojure id=8461bdc9-28a4-49c8-9f7e-d2b97cd37ef2
(vertex 1 [1/20 0] [1 -100])
```

In conclusion, the lowest altitude Guillermo will reach is $\-125$ meters relative to sea level.

A certain company's main source of income is a mobile app.

The company's annual profit (in millions of dollars) as a function of the app's price (in dollars) is modeled by

$P(x)=-2(x-3)(x-11)$

**What would be the company's profit if the app price is $0$ dollars?**

The company's profit if the app price is $0$ dollars is given by $P(0)$.

$\\begin{aligned} P( 0)&=-2( 0-3)( 0-11) \\\\ &=-2(-3)(-11) \\\\ &=-66 \\end{aligned}$

In conclusion, the company will have a profit of $\-66$ million dollars if the app price is $0$ dollars.

(In other words, the company would lose $66$ million dollars.)

```python id=15b44351-e0b3-4484-bfa5-b80ecf3d120c

```

Ricardo throws a stone off a bridge into a river below.

The stone's height (in meters above the water), $x$ seconds after Ricardo threw it, is modeled by

$w(x)=-5(x-8)(x+4)$

**How many seconds after being thrown will the stone reach its maximum height?**

```clojure id=6ccacb97-48d2-4ce7-b1ac-50902404d6cf
(vertex -5 [1 -8] [1 4])
```

**The stone will reach its maximum height after $2$ seconds.**

```python id=3ad77572-e182-4f3b-a03a-d1fb7391ceb6
(-1 + 7) / 2
```

[text/plain](https://nextjournal.com/data/QmZPBqcsz9gnT4Vt7CMAr2Pj1DmsVAQ5WxvwGk7198iKT8?content-type=text%2Fplain&filename=text%2Fplain)

An object is launched from a platform.

Its height (in meters), $x$ seconds after the launch, is modeled by

$h(x)=-5(x+1)(x-9)$

**How many seconds after launch will the object hit the ground?**

The object hits the ground when $h(x)=0$.

```clojure id=0c56769a-a94b-4506-9623-82dfb4e97eb2
(roots [1 1] [1 -9])
```

We found that $h(x)=0$ for $x=-1$ or $x=9$. Since $x=-1$ doesn't make sense in our context, the only reasonable answer is $x=9$.

**Therefore, the object will hit the ground after $9$ seconds.**

A hovercraft takes off from a platform.

Its height (in meters), $x$ seconds after takeoff, is modeled by

$h(x)=-(x-11)(x+3)$

**What is the height of the hovercraft at the time of takeoff?**

The height of the hovercraft at the time of takeoff is given by $h(0)$.

```clojure id=f9f9535f-4cc4-4f54-8410-b02b9101b5d0
(defn f-of [n f1 f2 x]
  (+ (* n
        (+ (* (first f1) x)
           (last f1))
        (+ (* (first f2) x)
           (last f2)))))

(f-of -1 [1 -11] [1 3] 0)
```

**In conclusion, the height of the hovercraft at the time of takeoff is $33$ meters.**

**How many seconds after takeoff will the hovercraft land on the ground?**

The hovercraft lands on the ground when $h(x)=0$.

```clojure id=8208d1fb-8885-465d-bbe2-d047360bacb5
(roots [1 -11] [1 3])
```

We found that $h(x)=0$ for $x=11$ or $x=-3$. Since $x=-3$ doesn't make sense in our context, the only reasonable answer is $x=11$.

**In conclusion, the hovercraft will land on the ground after $11$ seconds.**

The power generated by an electrical circuit (in watts) as a function of its current $c$ (in amperes) is modeled by

$P(c)=-15c(c-8)$

**What current will produce the maximum power?**

To find the current that will produce the maximum power, we need to find the vertex's $x$\-coordinate.

```clojure id=7f10c0c4-00b1-47c0-bf01-d4efea06e0aa
(vertex 1 [-15 0] [1 -8])
```

**In conclusion, the maximum power occurs when the current is $4$ amperes.**

<details id="com.nextjournal.article">
<summary>This notebook was exported from <a href="https://nextjournal.com">https://nextjournal.com</a></summary>

```edn nextjournal-metadata
{:nodes
 {"0c56769a-a94b-4506-9623-82dfb4e97eb2"
  {:compute-ref #uuid "c60f5e27-f3b6-4198-bf1a-4c644cfc67a4",
   :exec-duration 58,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "422d3182-c80a-4377-beae-b19742580baf"]},
  "15b44351-e0b3-4484-bfa5-b80ecf3d120c"
  {:kind "code",
   :runtime [:runtime "5ec67471-4dd1-432a-9b96-9c8afdee185f"]},
  "2b075bb7-349d-4ae2-9c63-a71e1ec386d6"
  {:kind "code",
   :runtime [:runtime "5ec67471-4dd1-432a-9b96-9c8afdee185f"]},
  "3ab0d5e5-b2b7-419b-82f1-b20053934e7e"
  {:compute-ref #uuid "0c6bcdfb-63e6-4ff4-8e6a-ed86109af404",
   :exec-duration 216,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "422d3182-c80a-4377-beae-b19742580baf"]},
  "3ad77572-e182-4f3b-a03a-d1fb7391ceb6"
  {:kind "code",
   :runtime [:runtime "5ec67471-4dd1-432a-9b96-9c8afdee185f"]},
  "422d3182-c80a-4377-beae-b19742580baf"
  {:environment
   [:environment
    {:article/nextjournal.id
     #uuid "5b45eb52-bad4-413d-9d7f-b2b573a25322",
     :change/nextjournal.id
     #uuid "5cd52af1-7a79-4804-a169-d6ffcdb6eb7a",
     :node/id "0ae15688-6f6a-40e2-a4fa-52d81371f733"}],
   :kind "runtime",
   :language "clojure",
   :type :nextjournal},
  "587e03c2-ec9c-40cf-bb98-a35a30d539fa"
  {:kind "code",
   :runtime [:runtime "5ec67471-4dd1-432a-9b96-9c8afdee185f"]},
  "5c1e52c0-4b86-4890-b1e3-7dbb9f2b6954"
  {:kind "code",
   :runtime [:runtime "5ec67471-4dd1-432a-9b96-9c8afdee185f"]},
  "5ec67471-4dd1-432a-9b96-9c8afdee185f"
  {:environment
   [:environment
    {:article/nextjournal.id
     #uuid "5b45e08b-5b96-413e-84ed-f03b5b65bd66",
     :change/nextjournal.id
     #uuid "5cc983f4-4ad6-43ee-abdc-44c24d0533ff",
     :node/id "0149f12a-08de-4f3d-9fd3-4b7a665e8624"}],
   :kind "runtime",
   :language "python",
   :name nil,
   :type :jupyter},
  "6ccacb97-48d2-4ce7-b1ac-50902404d6cf"
  {:compute-ref #uuid "edf1cf2e-c1e6-4dcd-b791-08403b0f5696",
   :exec-duration 86,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "422d3182-c80a-4377-beae-b19742580baf"]},
  "7f10c0c4-00b1-47c0-bf01-d4efea06e0aa"
  {:compute-ref #uuid "37324537-a0d0-4b4f-ac2d-3a9df4fad682",
   :exec-duration 69,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "422d3182-c80a-4377-beae-b19742580baf"]},
  "8208d1fb-8885-465d-bbe2-d047360bacb5"
  {:compute-ref #uuid "84141549-2a56-4467-9bb8-9302e92b8873",
   :exec-duration 72,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "422d3182-c80a-4377-beae-b19742580baf"]},
  "8461bdc9-28a4-49c8-9f7e-d2b97cd37ef2"
  {:compute-ref #uuid "4fefe4ac-fb1a-4487-940c-5b7395d614af",
   :exec-duration 139,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "422d3182-c80a-4377-beae-b19742580baf"]},
  "d8366b3f-5fea-45c0-9ce5-d549b6529b02"
  {:kind "code",
   :runtime [:runtime "5ec67471-4dd1-432a-9b96-9c8afdee185f"]},
  "f9f9535f-4cc4-4f54-8410-b02b9101b5d0"
  {:compute-ref #uuid "692a88a0-fbad-4266-a0a4-da070b96f6f2",
   :exec-duration 66,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "422d3182-c80a-4377-beae-b19742580baf"]}}}
```
</details>
