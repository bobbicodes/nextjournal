# Instaparse Calculator

```edn id=ffcf0396-b3f9-40e6-a0c2-654401879781 no-exec
{:deps
 {org.clojure/clojure {:mvn/version "1.10.0"}
  instaparse {:mvn/version "1.4.10"}}}
```

Adapted from the infix calculator in "[The AWK Programming Language](https://ia902309.us.archive.org/25/items/pdfy-MgN0H1joIoDVoIC7/The_AWK_Programming_Language.pdf)", Chapter 5, which all too conveniently uses a context-free grammar that we can pass right to [Instaparse](https://github.com/Engelberg/instaparse). All I've done here is expand it to support variables and implicit multiplication for equations.

```clojure id=d749281d-0b2f-4769-b47b-473fb69bb91d
(ns algae.core
  (:require [instaparse.core :as insta]))

(def equation
  (insta/parser
   "<equation> = left <'='> right
   left = expr+
   right = expr+
   <expr> = term | (expr '+' term) | (expr '-' term)
   <term> = factor | (term '*'? factor) | (term '/' factor)
   <factor> = number | variable | <'('> expr <')'>
   number = #'[0-9]+'
   variable = #'[A-Za-z]'"))
```

The Instaparse documentation already gave us a basic infix parser, and also demonstrates how to use`(" insta/transform")` to do the evaluation. But this grammar (from the AWK text:) "... captures not only the form of arithmetic expressions but also the precedences and associativities of the operators. For example, an `("expr")` is the sum or difference of `("term")`s, but a `("term")` is made up of `("factor")`s, which assures that multiplication and division are dealt with before addition or subtraction."

We'll use some problems from [Khan Academy](https://www.khanacademy.org/math/algebra/one-variable-linear-equations/alg1-one-step-add-sub-equations/e/one_step_equations):

**Solve the equation.**

$\\huge{20 = r + 11}$

First, here is our parse tree:

```clojure id=5ad79126-c1f2-42d7-bd5a-e08833e654ff
(equation "20=r+11")
```

What we want the solver to do (ultimately) is to analyze the tree and figure out which operation to perform. But first we'll just take this one as a given and make a function to solve it for $r$. Since one side is a single number and the other is $r$and a number, we just need to subtract the $11$from the $20$:

```clojure id=ac244ecf-f614-4845-87ab-94735558e43b
(defn left [s]
  (rest (first (equation s))))

(defn right [s]
  (rest (last (equation s))))

(defn solve [s]
  (let [left-num (Integer/parseInt (first (rest (first (left s)))))
        right-num (Integer/parseInt (last (last (right s))))
        right-var (last (first (right s)))]
    (str right-var " = " (- left-num right-num))))

(solve "20=r+11")
```

Cool... but needs to be generalized, obviously. Working with these nested vectors seems really awkward the way I'm trying to do it. I feel like I'm missing something, but let's just do some more and see how it goes, see which patterns emerge.

$\\huge{27 = 15 + n}$

See, this one is perfect because it just switches the order of the right terms. This forces us to write a function that will check that.

```clojure id=535ec441-a756-4fe2-9122-6bfa8fa4a6a4
(defn left-num [s]
  (cond
    (= :number (first (first (left s))))
    (Integer/parseInt (last (first (left s))))
    (= :number (first (last (left s))))
    (Integer/parseInt (last (last (left s))))
    :else nil))

(defn right-num [s]
  (cond
    (= :number (first (first (right s))))
    (Integer/parseInt (last (first (right s))))
    (= :number (first (last (right s))))
    (Integer/parseInt (last (last (right s))))
    :else nil))

(defn left-var [s]
  (cond
    (= :variable (first (first (left s))))
    (last (first (left s)))
    (= :variable (first (last (left s))))
    (last (last (left s)))
    :else nil))

(defn right-var [s]
  (cond
    (= :variable (first (first (right s))))
    (last (first (right s)))
    (= :variable (first (last (right s))))
    (last (last (right s)))
    :else nil))

(left-num "27=15+n")
```

Cool, now we can solve it like this:

```clojure id=42e3a151-8a9e-45bc-9a97-c18ac13a3dbe
(defn solve [s]
    (str (right-var s) " = " (- (left-num s) (right-num s))))

(solve "27=15+n")
```

```clojure id=fa8a97c1-8120-4726-add8-1920464db649
(solve "24=y+19")
```

$\\huge{p+12=30}$

```clojure id=38783ede-2991-44f5-9a36-283c2a44de5f
(defn solve [s]
  (if (left-var s)
    (str (left-var s) " = " (- (right-num s) (left-num s)))
    (str (right-var s) " = " (- (left-num s) (right-num s)))))

(solve "p+12=30")
```

$\\huge{x - 21 = 6}$

OK now subtraction! Let's extract the minus sign to find out what operation to perform:

```clojure id=9c0d5eb4-73c0-4c8f-8618-49599a931bdd
(defn operator [s]
  (if (= 3 (count (left s)))
    (second (left s))
    (second (right s))))

(operator "x-21=6")
```

```clojure id=16d5ecf2-e9c6-455f-965f-7c3ee93e515f
(defn solve [s]
  (if (left-var s)
    (str (left-var s) " = "
         (if (= "-" (operator s))
           (+ (right-num s) (left-num s))
           (- (right-num s) (left-num s))))
    (str (right-var s) " = "
         (if (= "-" (operator s))
           (+ (left-num s) (right-num s))
           (- (left-num s) (right-num s))))))

(solve "x-21=6")
```

```clojure id=b9911937-a30c-423d-8891-7606633e99cc
(solve "19=14+k")
```

<details id="com.nextjournal.article">
<summary>This notebook was exported from <a href="https://nextjournal.com">https://nextjournal.com</a></summary>

```edn nextjournal-metadata
{:nodes
 {"16d5ecf2-e9c6-455f-965f-7c3ee93e515f"
  {:compute-ref #uuid "03becc35-563a-4a8b-82b2-9b1ee666d0ee",
   :exec-duration 156,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
  "38783ede-2991-44f5-9a36-283c2a44de5f"
  {:compute-ref #uuid "85634f24-9ab2-4fe3-b7ee-a4c6e4f85592",
   :exec-duration 174,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
  "42e3a151-8a9e-45bc-9a97-c18ac13a3dbe"
  {:compute-ref #uuid "f982f697-5427-4560-9515-c3d33634ef1c",
   :exec-duration 96,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
  "535ec441-a756-4fe2-9122-6bfa8fa4a6a4"
  {:compute-ref #uuid "4b0014db-5e54-4520-a89b-cfe92121f16a",
   :exec-duration 140,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
  "5ad79126-c1f2-42d7-bd5a-e08833e654ff"
  {:compute-ref #uuid "9bafb13c-1bf7-46fe-95db-65082c817fcb",
   :exec-duration 56,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
  "80403b0a-1226-48ff-9bcc-624ed02e3635"
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
  "9c0d5eb4-73c0-4c8f-8618-49599a931bdd"
  {:compute-ref #uuid "36ed3392-cd19-488c-848e-7e965bb1a82d",
   :exec-duration 88,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
  "ac244ecf-f614-4845-87ab-94735558e43b"
  {:compute-ref #uuid "d1fd40c5-a948-4deb-890f-069a45f630ee",
   :exec-duration 70,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
  "b9911937-a30c-423d-8891-7606633e99cc"
  {:compute-ref #uuid "1cfb06b5-f58d-4062-a57e-85396808bd6a",
   :exec-duration 137,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
  "d749281d-0b2f-4769-b47b-473fb69bb91d"
  {:compute-ref #uuid "57ce9363-c526-413e-9ea4-53f543c8034a",
   :exec-duration 338,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
  "fa8a97c1-8120-4726-add8-1920464db649"
  {:compute-ref #uuid "090f0e79-5f84-46d3-9cfb-881b9a8e58cb",
   :exec-duration 106,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "80403b0a-1226-48ff-9bcc-624ed02e3635"]},
  "ffcf0396-b3f9-40e6-a0c2-654401879781" {:kind "code-listing"}}}
```
</details>
