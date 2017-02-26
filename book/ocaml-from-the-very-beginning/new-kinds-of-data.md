**Design a new type `rect` for representing rectangles. Treat squares as a
special case.**

```ocaml
type rect =
  | Rectangle of int * int
  | Square of int
;;
```

**Now write a function of type `rect -> int` to calculate the area of a given
`rect`.**

```ocaml
let area figure =
  match figure with
  | Rectangle (w, h) -> w * h
  | Square s -> s * s
;;
```

**Write a function which rotates a `rect` such that it is at least as tall as it
is wide.**

```ocaml
let rotate r =
  match r with
  | Rectangle (w, h) ->
    if w > h then Rectangle (h, w) else r
  | _ -> r
;;
```

**Use this function to write one which, given a `rect list`, returns another
such list which has the smallest total width and whose members are sorted
narrowest first.**

```ocaml
(* this requires quite a lot of boilerplate code! *)
let rec take n xs =
  match n, xs with
  | 0, _ | _, [] -> []
  | n', x :: xs' -> x :: take (n' - 1) xs'
;;

let rec drop n xs =
  match n, xs with
  | _, [] -> []
  | 0, xs' -> xs'
  | n', x :: xs' -> drop (n' - 1) xs'
;;

(* i tried to write merge_sort from scratch again, yeesh *)
let rec merge cmp xs ys =
  match xs, ys with
  | [], l | l, [] -> l
  | x :: xs', y :: ys' ->
    if cmp x y
      then x :: merge cmp xs' (y :: ys')
      else y :: merge cmp (x :: xs') ys'
;;

let rec merge_sort cmp xs =
  match xs with
  | [] -> []
  | [x] -> [x]
  | xs' ->
    let pivot = (List.length xs) / 2 in
      let left = take pivot xs in
        let right = drop pivot xs in
          merge cmp (merge_sort cmp left) (merge_sort cmp right)
;;

let compare rect_a rect_b =
  let width r =
    match r with
    | Rectangle (w, _) -> w
    | Square s -> s
  in
    width rect_a < width rect_b
;;

let sort_rectangles rects =
  merge_sort compare (List.map rotate rects)
;;
```

**Write `take`, `drop`, and `map` functions for the `sequence` type.**

```ocaml
(* sequence type, for reference *)
type 'a sequence = Nil | Cons of 'a * 'a sequence;;

let rec take n seq =
  match n, seq with
  | 0, _ | _, Nil -> Nil
  | n', Cons (head, tail) -> Cons (head, take (n' - 1) tail)
;;

let rec drop n seq =
  match n, seq with
  | _, Nil -> Nil
  | 0, seq' -> seq'
  | n', Cons (head, tail) -> drop (n' - 1) tail
;;

let rec map f seq =
  match seq with
  | Nil -> Nil
  | Cons (head, tail) -> Cons (f head, map f tail)
;;
```

**Extend the `expr` type and the `evaluate` function to allow raising a number
to a power.**

```ocaml
(* previous expr and evaluate, for reference *)
type expr =
  | Num of int
  | Add of expr * expr
  | Subtract of expr * expr
  | Multiply of expr * expr
  | Divide of expr * expr
  (* Added *)
  | Power of expr * expr
;;

let rec power x n =
  match n with
  | 0 -> 1
  | 1 -> x
  | _ -> x * power x (n - 1)
;;

let rec evaluate e = match e with
  | Num x -> x
  | Add (e, e') -> evaluate e + evaluate e'
  | Subtract (e, e') -> evaluate e - evaluate e'
  | Multiply (e, e') -> evaluate e * evaluate e'
  | Divide (e, e') -> evaluate e / evaluate e'
  (* Added *)
  | Power (e, e') -> power (evaluate e) (evaluate e')
;;
```

**Use the `option` type to deal with the problem that `Division_by_zero` may be
raised from the `evaluate` function.**

```ocaml
let safe_evaluate e =
  try
    Some (evaluate e)
  with
    Division_by_zero -> None
;;
```
