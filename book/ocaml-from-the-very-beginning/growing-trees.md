**Write a function of type `α → α tree → bool` to determine if a given element
is in a tree.**

```ocaml
type 'a tree =
  | Br of 'a * 'a tree * 'a tree
  | Lf

(* a -> a tree -> bool *)
let rec contains x tr =
  match tr with
  | Lf -> false
  | Br (y, left, right) -> x = y || contains x left || contains x right
;;
```

**Write a function which flips a tree left to right such that, if it were drawn
on paper, it would appear to be a mirror image.**

```ocaml
let rec flip tr =
  match tr with
  | Br (y, l, r) -> Br (y, flip r, flip l)
  | Lf -> tr
;;
```

**Write a function to determine if two trees have the same shape, irrespective
of the actual values of the elements.**

```ocaml
let rec same_shape tr tr' =
  match tr, tr' with
  | Br (_, left, right), Br (_, left', right') ->
    same_shape left left' && same_shape right right'
  | Lf, Lf -> true
  | _, _ -> false
;;
```

**Write a function `tree_of_list` which builds a `tree` representation of a
dictionary from a list representation of a dictionary.**

```ocaml
let rec insert (x_key, x_value) tr =
  match tr with
  | Lf -> Br ((x_key, x_value), Lf, Lf)
  | Br ((y_key, y_value), l, r) ->
    if x_key = y_key then tr
    else if x_key < y_key then Br ((y_key, y_value), insert (x_key, x_value) l, r)
    else Br ((y_key, y_value), l, insert (x_key, x_value) r)
;;

let rec tree_of_list xs =
  match xs with
  | [] -> Lf
  | x :: rest -> insert x (tree_of_list rest)
;;
```

**Write a function to combine two dictionaries represented as trees into one. In
the case of clashing keys, prefer the value from the first dictionary.**

```ocaml
let rec list_from_tree tr =
  match tr with
  | Lf -> []
  | Br (x, l, r) -> list_from_tree l @ [x] @ list_from_tree r
;;

let combine_trees d d' =
  tree_of_list (list_from_tree d @ list_from_tree d')
;;
```

**Can you define a type for trees which, instead of branching exactly two ways
each time, can branch zero or more ways, possibly different at each branch?
Write simple functions like `size`, `total`, and `map` using your new type of
tree.**

```ocaml
type 'a ntree = Branch of 'a * 'a ntree list;;

let rec map_ntree f (Branch (x, branches)) =
  Branch (f x, List.map (map_ntree f) branches)
;;

let rec sum xs =
  match xs with
  | [] -> 0
  | x :: rest -> 1 + sum rest
;;

let rec size (Branch (x, branches)) =
  1 + sum (List.map size branches)
;;

let rec total (Branch (x, branches)) =
  x + sum (List.map total branches)
;;
```
