**In `msort`, we calculate the value of the expression `length l / 2` twice.
Modify `msort` to remove this inefficiency.**

```ocaml
let rec merge x y =
  match x, y with
  | [], l -> l
  | l, [] -> l
  | hx::tx, hy::ty ->
      if hx < hy
        then hx :: merge tx (hy :: ty)
        else hy :: merge (hx :: tx) ty

let rec msort l =
  match l with
  | [] -> []
  | [x] -> [x]
  | _ ->
    let pivot = length / 2 in
      let left = take pivot l in
        let right = drop pivot l in
          merge (msort left) (msort right)
```

**We know that `take` and `drop` can fail if called with incorrect arguments.
Show that this is never the case in `msort`.**

```ocaml
(* take and drop for reference *)
let rec take n l =
  if n = 0 then [] else
    match l with
      h::t -> h :: take (n - 1) t

let rec drop n l =
  if n = 0 then l else
    match l with
      h::t -> drop (n - 1) t
```

> `drop` and `take` only fail if the `n` provided is longer than `length l`. But
> that will never happen with `msort` given that the `n` provided is always half
> of `length l`. So we can conclude that `n <= length l` for every call on
> `drop` and `take` in `msort`.

**Write a version of insertion sort which sorts the argument list into reverse
order.**

```ocaml
let rec insert x xs =
  match xs with
  | [] -> [x]
  | y::ys ->
    if x >= y
      then x :: y :: ys
      else y :: (insert x ys);;

let rec sort xs =
  match xs with
  | [] -> []
  | y::ys -> insert y (sort ys);;
```

**Write a function to detect if a list is already in sorted order.**

```ocaml
let rec is_sorted xs =
  match xs with
  | [] | [_] -> true
  | x::y::ys -> x <= y && is_sorted (y::ys)
```

**We mentioned that the comparison functions like `<` work for many OCaml types.
Can you determine, by experimentation, how they work for lists? For example,
what is the result of `[1; 2] < [2; 3]`? What happens when we sort the following
list of type char list list? Why?**

> It seems like the comparison is done between the heads of each list. If they
> happen to be equal then the heads of the tails get compared, and so on.

```ocaml
[['o'; 'n'; 'e']; ['t'; 'w'; 'o']; ['t'; 'h'; 'r'; 'e'; 'e']];;

(* would become *)

[['o'; 'n'; 'e']; ['t'; 'h'; 'r'; 'e'; 'e']; ['t'; 'w'; 'o']];;
```

**Combine the `sort` and `insert` functions into a single sort function.**

```ocaml
let rec insert_sort xs =
  let rec insert x xs =
    match xs with
    | [] -> [x]
    | y::ys ->
      if x <= y
        then x :: y :: ys
        else y :: (insert x ys)
  in
    match xs with
    | [] -> []
    | y::ys -> insert y (sort ys);;
```
