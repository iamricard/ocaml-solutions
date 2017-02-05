**Write a function `evens` which does the opposite to `odds`, returning the even
numbered elements in a list. For example, `evens [2; 4; 2; 4; 2]` should return
`[4; 4]`. What is the type of your function?**

```ocaml
(* 'a list -> 'a list *)
let rec evens xs =
  match xs with
  | _::y::ys -> y :: evens ys
  | _ -> [];;
```

**Write a function `count_true` which counts the number of `true` elements in a
list. For example, `count_true [true; false; true]` should return `2`. What is
the type of your function? Can you write a tail recursive version?**

```ocaml
(* bool list -> int *)
let count_true xs =
  let rec go xs c =
    match xs with
    | [] -> c
    | true::tl -> go tl (c + 1)
    | false::tl -> go tl c
  in
    go xs 0;;
```

**Write a function which, given a list, builds a palindrome from it. A
palindrome is a list which equals its own reverse. You can assume the existence
of `rev` and `@`. Write another function which determines if a list is a
palindrome.**

```ocaml
let make_palindrome xs =
  xs @ (rev xs);;

let is_palindrome xs =
  xs = (rev xs);;
```

**Write a function `drop_last` which returns all but the last element of a list.
If the list is empty, it should return the empty list. So, for example,
`drop_last [1; 2; 4; 8]` should return `[1; 2; 4]`. What about a tail recursive
version?**

```ocaml
let rec drop_last xs =
  match xs with
  | [] | [_] -> []
  | y::ys -> y :: drop_last ys;;

let drop_last_opt xs =
  let rec go xs acc =
    match xs with
    | [] | [_] -> rev acc
    | y::ys -> go ys (y :: acc)
  in
    go xs [];;
```

**Write a function member of type `α → α list → bool` which returns `true` if an
element exists in a list, or `false` if not. For example, `member 2 [1; 2; 3]`
should evaluate to `true`, but `member 3 [1; 2]` should evaluate to `false`.**

```ocaml
let rec member t xs =
  match xs with
  | [] -> false
  | y::ys -> y = t || member t ys;;
```

**Use your member function to write a function `make_set` which, given a list,
returns a list which contains all the elements of the original list, but has no
duplicate elements. For example, `make_set [1; 2; 3; 3; 1]` might return
`[2; 3; 1]`. What is the type of your function?**

```ocaml
let make_set xs =
  let rec go xs set =
    match xs with
    | [] -> set
    | y::ys -> if member y set then go ys set else go ys (y :: set)
  in
    go xs [];;
```

**Can you explain why the `rev` function we defined is inefficient? How does the
time it takes to run relate to the size of its argument? Can you write a more
efficient version using an accumulating argument? What is its efficiency in
terms of time taken and space used?**

```ocaml
(* Provided `rev`, for reference. *)
let rec rev l = match l with
  | [] -> []
  | h::t -> rev t @ [h];;
```

> We know that the `@` operator takes `O(n)` time to run, where `n` is the
> length of the list on the left-hand side (this is told to us on page 26, and
> _proved_ on page 29). So we will be running an `O(n)` operation `n` times. So,
> I think, the time complexity is `O(n^2)`. As for space complexity it would be
> `O(n^2)` as well, I think.

```ocaml
(* I think both time and space should be O(n) *)
let rev_opt xs =
  let rec go xs acc =
    match xs with
    | [] -> []
    | y::ys -> go ys (y :: acc)
  in
    go xs [];;
```
