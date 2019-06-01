# Solving equations

## [One-step addition & subtraction equations: fractions & decimals](https://www.khanacademy.org/math/algebra/one-variable-linear-equations/alg1-one-step-add-sub-equations/e/one-step-add-sub-equations-2)

**Solve the equation.**

$\\huge{\\dfrac13 + a = \\dfrac54}$

$\\large{a=}$

```clojure id=6b11808b-a5c6-4178-ad20-81ecf36c0a73
(require '[clojure.string :as str])

(defn round [n]
  (if (float? n)
    (float (/ (Math/round (* n 100)) 100)) n))

(defn parse-term [s]
  (if (Character/isDigit (first s))
    (if (str/includes? s ".")
      (Float/parseFloat s)
      (Integer/parseInt s)) (symbol s)))

(defn term1 [s op]
  (parse-term (str/trim (first (str/split s op)))))
(defn term2 [s op]
  (parse-term (str/trim (last (str/split s op)))))

(defn num? [s]
  (if-let [s (seq s)]
    (let [s (if (= (first s) \-) (next s) s)
          s (drop-while #(Character/isDigit %) s)
          s (if (= (first s) \.) (next s) s)
          s (drop-while #(Character/isDigit %) s)]
      (empty? s))))

(defn div [s]
  (let [left (first (str/split s #"/"))
        right (last (str/split s #"/"))]
    (cond
      (and (num? left) (num? right))
      (/ (term1 s #"/") (term2 s #"/"))
      (str/includes? left "-")
      [(symbol (first (str/split left #"\-")))
       (- (/ (parse-term (last (str/split left #"\-")))
             (term2 s #"/")))]
      (str/includes? right "-")
      [(symbol (last (str/split right #"\-")))
       (/ (term1 s #"/")
          (parse-term (first (str/split right #"\-"))))]
      (str/includes? right "+")
      [(/ (parse-term left)
        (parse-term (first (str/split right #"\+"))))
       (symbol (last (str/split right #"\+")))]
      (str/includes? left "+")
      [(symbol (first (str/split left #"\+")))
       (/ (parse-term (last (str/split left #"\+")))
          (term2 s #"/"))]
      :else {:numer left :denom right})))

(defn parse-exp [s]
  (cond
    (re-matches #"(\d+\.?\d*|\d*\.?\d+)[a-zA-Z]" s)
    {:variable (re-find #"[a-zA-Z]" s)
     :multiplier (parse-term (first (re-find #"(\d+\.?\d*|\d*\.?\d+)" s)))}
    (str/includes? s "*")
    (* (term1 s #"\*") (term2 s #"\*"))
    (str/includes? s "/")
    (div s)
    (str/includes? s "+")
    [(term1 s #"\+") (term2 s #"\+")]
    (str/includes? s "-")
    (if (symbol? (term2 s #"\-"))
      [(term1 s #"\-")
       (symbol (str "-" (term2 s #"\-")))]
      [(term1 s #"\-")
       (if (number? (term2 s #"\-"))
         (- (term2 s #"\-")) (term2 s #"\-"))])
    :else (parse-term (str/trim s))))

(defn parse-eq [s]
  (let [left  (str/trim (first (str/split s #"=")))
        right (str/trim (last (str/split s #"=")))]
    {:left (parse-exp left) :right (parse-exp right)}))

(defn solve [s]
  (let [left (:left (parse-eq s))
        right (:right (parse-eq s))]
    (cond
      (:numer left)
      (str (:numer left) " = "
           (round (* (parse-term (:denom left)) right)))
      (:numer right)
      (str (:numer right) " = "
           (round (* (parse-term (:denom right)) left)))
      (:multiplier left)
      (str (:variable left) " = "
           (round (/ right (:multiplier left))))
      (:multiplier right)
      (str (:variable right) " = "
           (round (/ left (:multiplier right))))
      (number? right)
      (if (number? (last left))
        (str (first left) " = "
             (round (- right (last left))))
        (if (= "-" (str (first (str (last left)))))
          (str (str/replace (last left) "-" "") " = "
               (round (- (- right (first left)))))
          (str (last left) " = "
               (round (- right (first left))))))
      (number? left)
      (if (number? (last right))
        (str (first right) " = "
             (round (- left (last right))))
        (if (= "-" (str (first (str (last right)))))
          (str (str/replace (last right) "-" "") " = "
               (round (- (- left (first right)))))
          (str (last right) " = "
               (round (- left (first right)))))))))

(solve "17=r+10")
```

<details id="com.nextjournal.article">
<summary>This notebook was exported from <a href="https://nextjournal.com">https://nextjournal.com</a></summary>

```edn nextjournal-metadata
{:nodes
 {"6976a4c8-3ec9-4a4e-a01f-3fa1617de1d5"
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
  "6b11808b-a5c6-4178-ad20-81ecf36c0a73"
  {:compute-ref #uuid "438f776d-04c4-40a8-98af-1812db14457e",
   :exec-duration 124,
   :kind "code",
   :output-log-lines {},
   :refs (),
   :runtime [:runtime "6976a4c8-3ec9-4a4e-a01f-3fa1617de1d5"]}}}
```
</details>
