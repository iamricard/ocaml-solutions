**Write a simple recursive function calm to replace exclamation marks in a char
list with periods. For example
`calm ['H'; 'e'; 'l'; 'p'; '!'; ' '; 'F'; 'i'; 'r'; 'e'; '!']` should evaluate
to `calm ['H'; 'e'; 'l'; 'p'; '.'; ' '; 'F'; 'i'; 'r'; 'e'; '.']`. Now rewrite
your function to use map instead of recursion. What are the types of your
functions?**

```ocaml
(* char list -> char -> list *)
let rec calm_recursive xs =
  match xs with
  | [] -> []
  | '!'::ys -> '.' :: calm_recursive ys
  | y::ys -> y :: calm_recursive ys
;;

(* char -> char *)
let exclamation_to_period c =
  match c with
  | '!' -> '.'
  | a -> a
;;

(* char list -> char -> list *)
let calm =
  List.map exclamation_to_period
;;
```

**Write a function `clip` which, given an `integer`, clips it to the range
`1...10` so that integers bigger than `10` round down to `10`, and those smaller
than `1` round up to `1`. Write another function `cliplist` which uses this
first function together with `map` to apply this clipping to a whole `list` of
integers.**

```ocaml
let clip n =
  if n > 10 then 10 else
    if n < 1 then 1 else
      n
;;

let cliplist =
  List.map clip
;;
```

**Express your function `cliplist` again, this time using an anonymous function
instead of `clip`.**

```ocaml
let cliplist =
  List.map (fun n -> if n > 10 then 10 else if n < 1 then 1 else n)
;;
```

**Write a function `apply` which, given another function, a number of times to
apply it, and an initial argument for the function, will return the cumulative
effect of repeatedly applying the function. For instance, `apply f 6 4` should
return `f (f (f (f (f (f 4))))))`. What is the type of your function?**

```ocaml
(* (a -> a) -> int -> a -> a *)
let rec apply f n x =
  if n = 0 then x else
    f (apply f (n - 1) x)
;;
```

**Modify the insertion sort function from the preceding chapter to take a
comparison function, in the same way that we modified merge sort in this
chapter. What is its type?**

```ocaml
(* (a -> a -> bool) -> a -> a list -> a list *)
let rec insert cmp x xs =
  match xs with
  | [] -> [x]
  | y::ys ->
    if cmp x y
      then x :: y :: ys
      else y :: (insert x ys)
;;

(* (a -> a -> bool) -> a -> a list *)
let rec insert_sort cmp xs =
  match xs with
  | [] -> []
  | y::ys -> insert cmp y (sort ys)
;;
```

**Write a function filter which takes a function of type `α → bool` and an `α
list` and returns a list of just those elements of the argument list for which
the given function returns `true`.**

```ocaml
let rec filter f xs =
  match xs with
  | [] -> []
  | y::ys ->
    if f y
      then y :: filter f ys
      else filter f ys
;;
```

**Write the function `for_all` which, given a function of type `α → bool` and an
argument list of type `α list` evaluates to `true` if and only if the function
returns `true` for every element of the list. Give examples of its use.**

```ocaml
let rec for_all f xs =
  match xs with
  | [] -> true
  | y::ys -> f y && for_all f ys
;;

for_all (fun x -> x > 1) [1; 3; 4; 5];; (* false *)
for_all (fun x -> x > 1) [2; 3; 4; 5];; (* true *)
```

**Write a function `mapl` which maps a function of type `α → β` over a `list` of
type `α list list` to produce a `list` of type `β list list`.**

```ocaml
let rec mapl f =
  List.map (List.map f)
;;
```
