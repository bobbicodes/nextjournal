# Quadratic functions

## Factored form

$\\Large{f(x)=(x+2)(x-4)}$

**What are the zeros of the function?**

```clojure id=b5509561-740f-4ba8-9350-4db505031952
(defn root [[b c]]
  (- (/ c b)))

(defn roots [eq1 eq2]
  [(root eq1) (root eq2)])

(roots [1 2] [1 -4])

(defn average [x y]
  (/ (+ x y) 2))

(defn factored-form [[n1 n2 n3]]
  (let [roots (roots [1 n2] [1 n3])
        vert-x (apply average roots)]
    {:roots roots
     :vertex [vert-x (* n1
                        (+ vert-x n2)
                        (+ vert-x n3))]}))

(factored-form [-1 -12 3])
```

**What is the vertex of the parabola?**

## Vertex form

In general, for any quadratic function written in vertex form $f(x)=a(x-\\redD{h})^2+\\greenD{k}$ we can conclude that the vertex is the point $(\\redD{h},\\greenD{k})$.

**The vertex of the graph of **

$m(x)=4(x-\\redD{1})^2\\greenD{-36}$** is the point **$(\\redD{1},\\greenD{-36})$**.**

$\\Large{h(x)=(x+5)^2-12.25}$

**What are the zeros of the function?**

```clojure id=4285fc86-6011-4369-934e-b4d28256815d
(defn vertex-form [[n1 n2 n3]]
  {:roots [(+ (- n2) (Math/sqrt (* n1 (- n3))))
           (- (- n2) (Math/sqrt (* n1 (- n3))))]
   :vertex [(- n2) n3]})
  
(vertex-form [1 -6 -12.25])
```

$\\begin{aligned} (x+5)^2-12.25&=0 \\\\\\\\ (x+5)^2&=12.25 \\\\\\\\ \\sqrt{(x+5)^2}&=\\sqrt{12.25} \\\\\\\\ x+5&=\\pm 3.5 \\\\\\\\ x&=\\pm 3.5-5 \\\\\\\\ x=\\goldD{-8.5}&\\text{ or }x=\\goldD{-1.5} \\end{aligned}$

**What is the vertex of the parabola?**

## Standard form

$\\Large{g(x)=x^2+5x-14}$

**What are the zeros of the function?**

**What is the vertex of the parabola?**

```clojure id=8fc4645b-af05-4253-9b83-0b74b7f1a9bb
(defn factors [n]
  (for [x (into (range (- (Math/abs n)) 0)
                (range 1 (inc (Math/abs n))))
        :when (zero? (rem n x))]
    [(int x) (int (/ n x))]))

(defn zeros [[a b c]]
  (map - (first (filter #(= b (apply + %))
                        (factors c)))))

(defn div-by-a [s]
  (map #(/ % (first s)) s))

(defn vert-x [[a b c]]
  (apply average
       (-> [a b c]
         div-by-a
         zeros)))

(defn solve [[a b c] x]
  (+ (* (* x x) a)
     (* b x)
     c))

(defn vertex [[a b c :as eq]]
  (let [x (apply average (zeros eq))
        y (solve eq x)]
    [x y]))

(defn standard-form [[a b c]]
  {:roots (zeros [a b c])
   :vertex (vertex [a b c])})

(standard-form [1 4 -60])
```

<details id="com.nextjournal.article">
<summary>This notebook was exported from <a href="https://nextjournal.com">https://nextjournal.com</a></summary>

```edn nextjournal-metadata
{:nodes
 {"4285fc86-6011-4369-934e-b4d28256815d"
  {:compute-ref #uuid "c03bfd5b-902c-4b4c-abee-bfa8a9efc358",
   :exec-duration 87,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e5a2a45b-f76d-40b6-89aa-88dc8247479a"]},
  "8fc4645b-af05-4253-9b83-0b74b7f1a9bb"
  {:compute-ref #uuid "e56dd22a-0b5d-4c48-8b05-e69d129b4142",
   :exec-duration 138,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e5a2a45b-f76d-40b6-89aa-88dc8247479a"]},
  "b5509561-740f-4ba8-9350-4db505031952"
  {:compute-ref #uuid "4dd3ca1c-8a0d-4a7b-9ae7-76fca75e2ce0",
   :exec-duration 127,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "e5a2a45b-f76d-40b6-89aa-88dc8247479a"]},
  "e5a2a45b-f76d-40b6-89aa-88dc8247479a"
  {:environment
   [:environment
    {:article/nextjournal.id
     #uuid "5b45eb52-bad4-413d-9d7f-b2b573a25322",
     :change/nextjournal.id
     #uuid "5cd52af1-7a79-4804-a169-d6ffcdb6eb7a",
     :node/id "0ae15688-6f6a-40e2-a4fa-52d81371f733"}],
   :kind "runtime",
   :language "clojure",
   :type :nextjournal}}}
```
</details>
