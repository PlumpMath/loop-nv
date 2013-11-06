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
expression. The names must be of local bindings defined in the <code>loop-nv</code> expression. The names will be bound to their associated values in the next loop. Names that do not occur in the <code>recur-nv</code> expression will have the same values in the next loop. The same name may appear in a name-value pair more than once, in which case the rightmost pair takes priority.

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

## License

Copyright © 2013 Timothy Gardner

Distributed under the Eclipse Public License.
