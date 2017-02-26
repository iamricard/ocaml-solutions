**Rewrite the summary paragraph at the end of this chapter for the three
argument function `g a b c`.**

```ocaml
let g = fun a -> fun b -> fun c -> ...
```

**Recall the function `member x l` which determines if an element `x` is
contained in a `list l`. What is its type? What is the type of `member x`? Use
partial application to write a function `member_all x ls` which determines if an
element is a member of all the lists in the list of lists `ls`.**

```ocaml
(* a -> a list -> bool *)
let rec member t xs =
  match xs with
  | [] -> false
  | y::ys -> y = t || member t ys
;;

let member_all x ls =
  let rec go matches =
    match matches with
    | [] -> true
    | b :: rest -> b && go rest
  in
    go (List.map (member x) ls)
;;
```

**Why can we not write a function to halve all the elements of a list like this:
`map (( / ) 2) [10; 20; 30]`? Write a suitable division function which can be
partially applied in the manner we require.**

> We can't write like that because the first argument of the `/` operator is the
> numerator, rather than the dividend. So this would end up in having `2` be
> divided by every member of the list instead.

```ocaml
let flipped_division d n =
  n / d
;;
```

**Write a function `mapll` which maps a function over lists of lists of lists.
You must not use the `let rec` construct. Is it possible to write a function
which works like `map`, `mapl`, or `mapll` depending upon the list given to
it?**

```ocaml
let mapll f =
  List.map (List.map (List.map f))
;;
```

**Write a function `truncate` which takes an integer and a list of lists, and
returns a list of lists, each of which has been truncated to the given length.
If a list is shorter than the given length, it is unchanged. Make use of partial
application.**

```ocaml
let truncate n =
  let rec f n l =
    match n, l with
    | 0, _ | _, [] -> []
    | n, x :: rest -> x :: f (n - 1) rest
  in
    List.map (f n)
;;
```

**Write a function which takes a list of lists of integers and returns the list
composed of all the first elements of the lists. If a list is empty, a given
number should be used in place of its first element.**

```ocaml
let head (x :: _) = x;;

let heads = List.map head;;
```
