# loop-nv

Named arguments for recursion in loops.

## Rationale

Positional arguments to <code>recur</code> make <code>loop</code> expressions even harder to understand.
Named arguments seem to help in many cases.

## Usage

```clojure
(use '[loop-nv.loop-nv :only [loop-nv recur-nv]])
```

<code>loop-nv</code> is identical to <code>loop</code>, except it admits <code>recur-nv</code> expressions in addition to normal <code>recur</code> expressions. 
 
<code>recur-nv</code> takes a series of name-value pairs, like bindings in a <code>let</code>
expression. The names must be of local bindings defined in the <code>loop-nv</code> expression. The names will be bound to their associated values in the next loop. Names that do not occur in the <code>recur-nv</code> expression will have the same values in the next loop. 

```clojure
 (loop-nv [x 0, y 10, loop-count 0]
   (let [diff (- x y)]
     (cond (zero? diff)
           [x y loop-count]
		       
           (odd? diff)
           (recur-nv loop-count (inc loop-count)
                     x (inc x))
		       
           :else
           (recur-nv loop-count (inc loop-count) 
                     y (dec y)))))
 ;; => [5 5 10]
```

Works nicely with clojure.tools.macro/macrolet:

```clojure
;; using neat external library: https://github.com/clojure/tools.macro
(in-ns 'loop-nv.loop-nv)
(ns macrolet-demo
  (:use [loop-nv.loop-nv :only [loop-nv recur-nv]]
        [clojure.tools.macro :only [macrolet]]
        [clojure.pprint :only [pprint]]))

(pprint
 (loop-nv [x 0,
           bldg [],
           mode :mode-a]
   (macrolet [(to-mode-a [& nvs]
                (concat '(recur-nv mode :mode-a
                                   x (inc x))
                        nvs))
              (to-mode-b [& nvs]
                (concat '(recur-nv mode :mode-b
                                   x (inc x))
                        nvs))]
     (if (< 10 x)
       bldg
       (case mode
         :mode-a
         (to-mode-b bldg (conj bldg [:a x]))
         
         :mode-b
         (to-mode-a bldg (conj bldg [:b x])))))))
```

This prints:

```clojure
[[:a 0]
 [:b 1]
 [:a 2]
 [:b 3]
 [:a 4]
 [:b 5]
 [:a 6]
 [:b 7]
 [:a 8]
 [:b 9]
 [:a 10]]
```

Name-value pairs may share names in the same recur expression, in which case the rightmost pair takes priority:






## License

Copyright © 2013 Timothy Gardner

Distributed under the Eclipse Public License.
