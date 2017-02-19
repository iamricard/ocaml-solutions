**Rewrite the `not` function from the previous chapter in pattern matching
style.**

```ocaml
let not b =
  match b with
  | true -> false
  | _ -> true
;;
```

**Use pattern matching to write a recursive function which, given a positive
integer `n`, returns the sum of all the integers from `1 to n`.**

```ocaml
let rec sum_up_to n =
  match n > 0 with
  | true -> n + sum_up_to (n - 1)
  | _ -> 0
;;
```

**Use pattern matching to write a function which, given two numbers `x` and `n`,
computes `x^n`.**

```ocaml
(* won't work for negative exponents *)
let rec power x n =
  match n with
  | 0 -> 1
  | 1 -> n
  | _ -> x * power x (n - 1)
;;
```

**For each of the previous three questions, comment on whether you think it is
easier to read the function with or without pattern matching. How might you
expect this to change if the functions were much larger?**

> I think that, for the most part, it is easier to read with pattern matching.
> Although for `not` and `sum_up_to`, which only have two cases, might be
> overkill.

**What does `match 1 + 1 with 2 -> match 2 + 2 with 3 -> 4 | 4 -> 5` evaluate
to?**

```ocaml
5;;
```

**There is a special pattern `x..y` to denote continuous ranges of characters,
for example 'a'..'z' will match all lowercase letters. Write functions
`is_lower` and `is_upper`, each of type `char → bool`, to decide on the case of
a given letter.**

```ocaml
let is_lower c =
  match c with
  | 'a'..'z' -> true
  | _ -> false
;;

let is_upper c =
  match c with
  | 'A'..'Z' -> true
  | _ -> false
;;
```
