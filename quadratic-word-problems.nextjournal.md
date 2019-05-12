# Quadratic word problems

## Fish and water temperature

The number of fish is a function of the water's temperature:

$p(x)=-2x^2+40x-72$

```python id=0332947c-550f-402d-a931-08334ff96cb7
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(2, 18, 0.01)
y = -2*x**2 + 40*x - 72

fig, ax = plt.subplots()
ax.plot(x, y)
ax.set(xlabel='Water temperature', ylabel='Number of fish')
ax.grid()
plt.gcf()
```

**Which temperatures will result in **$0$** fish?**

There are no fish when $p(x)=0$.

To find the zeros, we need to factor the equation as a product of 2 binomials. First express it in standard form:

```clojure id=31ca9b13-c15e-4989-b4a1-d0b18089333f
(defn divide-by-a [[a b c r]]
  [1 (/ b a) (/ c a) (/ r a)])

(divide-by-a [-2 40 -72 0])
```

$x^2-20x+36=0$

We want to find the positive and negative integer factors of 36:

```clojure id=3a270ac3-4048-43e4-9c97-3943b6c7b52f
(defn factors [n]
      (for [x (into (range (- (inc (Math/abs n))) 0)
                    (range 1 (inc (Math/abs n))))
            :when (zero? (rem (Math/abs n) x))]
        [(int x) (int (/ n x))]))

(factors -24)
```

Now find the ones that add up to $\-20$and negate them to get the zeros:

```clojure id=f99ca4ff-8bf0-460c-8043-7639641f483b
(defn zeros [[a b c r]]
  (map - (first (filter #(= b (apply + %))
                        (factors c)))))

(-> [-2 40 -72 0]
  divide-by-a
  zeros)
```

## Profit and price of an item

Profit is a function of the price of an item:

$P(x)=-2x^2+16x-24$

```python id=b6d33576-bdce-4b60-a77a-18bd777c3483
x = np.arange(2, 6, 0.01)
y = -2*x**2 + 16*x - 24

fig, ax = plt.subplots()
ax.plot(x, y)
ax.set(xlabel='Price', ylabel='Profit')
ax.grid()
plt.gcf()
```

**What price results in the maximum profit?**

Find the vertex's $x$\-coordinate by taking the average of the two zeros:

```clojure id=c675ce95-f97f-45a1-a3ad-c9d143c1a1aa
(defn average [x y]
  (/ (+ x y) 2))

(defn vert-x [[a b c r]]
  (apply average
       (-> [a b c r]
         divide-by-a
         zeros)))

(vert-x [-2 16 -24 0])
```

## Height of a thrown stone

Alain throws a stone off a bridge into a river below.

The stone's height, $x$ seconds after Alain threw it, is modeled by:

$h(x)=-5x^2+10x+15$

```python id=e9f4cea0-61b8-4017-9897-480daaebe979
x = np.arange(0, 3, 0.01)
y = -5*x**2 + 10*x + 15
fig, ax = plt.subplots()
ax.plot(x, y)
ax.set(xlabel='Seconds', ylabel='Height')
ax.grid()
plt.gcf()
```

**What is the height of the stone at the time it is thrown?**

That's easy, substitute $0$for $x$ to get $15$.

## Height after takeoff

The spaceship's height (in meters), $x$ seconds after takeoff, is modeled by:

$h(x)=-2x^2+20x+48$

```python id=bdad01c0-1b0a-4126-908b-fb89f95ba0c9
a = -2; b = 20; c = 48;

x = np.arange(0, 5, 0.01)
y = a*x**2 + b*x + c
fig, ax = plt.subplots()
ax.plot(x, y)
ax.set(xlabel='Seconds', ylabel='Height')
ax.grid()
plt.gcf()
```

**What is the maximum height that the ship will reach?**

```clojure id=7bc9d87b-3ca6-4e4c-82e9-d64b979b1f4a
(vert-x [-2 20 48 0])
```

The vertex's $x$\-coordinate is $\\blueD 5$.

Now find $h(\\blueD{5})$:

```clojure id=57ea7e6e-7dd6-444b-ac1c-9edfdb1d7c3c
(defn solve [[a b c r] x]
  (+ (* (* x x) a)
     (* b x)
     c))

(solve [-2 20 48 0] 5)
```

<details id="com.nextjournal.article">
<summary>This notebook was exported from <a href="https://nextjournal.com">https://nextjournal.com</a></summary>

```edn nextjournal-metadata
{:nodes
 {"0332947c-550f-402d-a931-08334ff96cb7"
  {:compute-ref #uuid "b9964964-6a5b-4d81-b3cb-0e93bb363410",
   :exec-duration 1291,
   :kind "code",
   :output-log-lines {:stdout 0},
   :refs (),
   :runtime [:runtime "6444c6b4-a50a-4f66-a7ad-1361b2d481d7"]},
  "31ca9b13-c15e-4989-b4a1-d0b18089333f"
  {:compute-ref #uuid "a30b2bc2-6bd9-4374-8b09-5e0d1fbf2dd3",
   :exec-duration 85,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "3a270ac3-4048-43e4-9c97-3943b6c7b52f"
  {:compute-ref #uuid "3a238f82-7578-41eb-8d38-be9313678bea",
   :exec-duration 67,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "57ea7e6e-7dd6-444b-ac1c-9edfdb1d7c3c"
  {:compute-ref #uuid "1467c80f-0215-43fc-8ef7-cfa965809c75",
   :exec-duration 175,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "6444c6b4-a50a-4f66-a7ad-1361b2d481d7"
  {:environment
   [:environment
    {:article/nextjournal.id
     #uuid "5b45e08b-5b96-413e-84ed-f03b5b65bd66",
     :change/nextjournal.id
     #uuid "5cc983f4-4ad6-43ee-abdc-44c24d0533ff",
     :node/id "0149f12a-08de-4f3d-9fd3-4b7a665e8624"}],
   :kind "runtime",
   :language "python",
   :type :nextjournal},
  "7bc9d87b-3ca6-4e4c-82e9-d64b979b1f4a"
  {:compute-ref #uuid "d3e03649-aacc-460f-b890-65690b9c12c9",
   :exec-duration 23,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "b6d33576-bdce-4b60-a77a-18bd777c3483"
  {:compute-ref #uuid "f03a3f59-778b-46e4-ba88-45ffc91c1cb6",
   :exec-duration 1348,
   :kind "code",
   :output-log-lines {:stdout 0},
   :refs (),
   :runtime [:runtime "6444c6b4-a50a-4f66-a7ad-1361b2d481d7"]},
  "bdad01c0-1b0a-4126-908b-fb89f95ba0c9"
  {:compute-ref #uuid "1e95b676-187a-4cc7-b3b7-8c20b838a4a6",
   :exec-duration 1150,
   :kind "code",
   :output-log-lines {:stdout 0},
   :refs (),
   :runtime [:runtime "6444c6b4-a50a-4f66-a7ad-1361b2d481d7"]},
  "c675ce95-f97f-45a1-a3ad-c9d143c1a1aa"
  {:compute-ref #uuid "dec5778f-a18a-4152-9f27-6f18a3a1f893",
   :exec-duration 75,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]},
  "e1d916c0-0392-47e0-9fe7-bc184f741f59"
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
  "e9f4cea0-61b8-4017-9897-480daaebe979"
  {:compute-ref #uuid "a97ba075-a3f8-4917-83b3-eb3f817c857f",
   :exec-duration 1272,
   :kind "code",
   :output-log-lines {:stdout 0},
   :refs (),
   :runtime [:runtime "6444c6b4-a50a-4f66-a7ad-1361b2d481d7"]},
  "f99ca4ff-8bf0-460c-8043-7639641f483b"
  {:compute-ref #uuid "05bf20c9-89d5-4d6f-be73-1f0ee68f678b",
   :exec-duration 108,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e1d916c0-0392-47e0-9fe7-bc184f741f59"]}}}
```
</details>
