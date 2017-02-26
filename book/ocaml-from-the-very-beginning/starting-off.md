**What are the types of the following expressions and what do they evaluate to,
and why?**

```ocaml
let seventeen : int = 17;;
let eleven : int = 1 + 2 * 3 + 4;; (* 11 *)
let one : int = 800 / 80 / 8;; (* 1 *)
let a : bool = (400 > 200) = true;;
let b : bool = (1 <> 1) = false;;
let c : bool = (true || false) = true;;
let d : bool = (true && true) = true;;
let e : bool = (if true then false else true) = false;;
let f : char = '%';;
(* 'a' + 'b' -> Error! *)
```

**Consider the evaluations of the expressions `1 + 2 mod 3`, `(1 + 2) mod 3`,
and `1 + (2 mod 3)`. What can you conclude about the `+` and `mod` operators?**

> (mod) has higher precendece than (+).


**A programmer writes `1+2 * 3+4`. What does this evaluate to? What advice
would you give them?**

```ocaml
let expr : bool = (1+2 * 3+4) = 11;;

(* my suggestion would be to rewrite as follows *)
let expr' : bool = ((1 + 2) * (3 + 4)) = 21;;
```

**The range of numbers available is limited. There are two special numbers:
`min_int` and `max_int`. What are their values on your computer? What happens
when you evaluate the expressions `max_int + 1` and `min_int - 1`?**

```ocaml
min_int = (-4611686018427387904);;
max_int = 4611686018427387903;;

(* when we add to max_int or subtract from min_int, we loop around *)
(min_int - 1) = max_int;;
min_int = (max_int + 1);;
```

**What happens when you try to evaluate the expression 1 / 0? Why?**

> We get an Exception, because we can't divide by zero.

**Can you discover what the `mod` operator does when one or both of the
operands are negative? What about if the first operand is zero? What if the
second is zero?**

> Hmmm...

**Why not just use, for example, the integer 0 to represent false and the
integer 1 for true? Why have a separate bool type at all?**

> Because then we limit the range of possible inputs two `bool`, instead of
`int` which has a ridiculous amount of possibilities.

**What is the effect of the comparison operators like < and > on alphabetic
values of type char? For example, what does 'p' < 'q' evaluate to? What is the
effect of the comparison operators on the booleans, true and false?**

> For `bool` values, `true` is GT `false`. For `char` values it checks for
order. i.e. 'a' comes before 'g', therefor LT.
