# loop-nv

Named arguments for recursion in loops.

## Rationale

Positional arguments to recur make loop expressions even harder to understand.
Named arguments seem to help in many cases.

## Usage

loop-nv is identical to loop, except it admits recur-nv expressions in addition
to normal recur expressions. 
 
recur-nv takes a series of name-value pairs, like bindings in a let
expression. The names must be of local bindings defined in the loop-nv
expression. The values associated with those names in the recur-nv 
expression will be bound to those names in the next loop. Names that 
do not occur in the recur-nv expression will remain the same in the 
next loop. The same name may appear in a name-value pair more than
once, in which case the rightmost pair takes priority.

```clojure
 (loop-nv [x 0, y 10, loop-count 0]
   (let [diff (Math/abs (- x y))]
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

Copyright © 2013 FIXME

Distributed under the Eclipse Public License, the same as Clojure.
