**Write a function `smallest` which returns the smallest positive element of a
list of integers. If there is no positive element, it should raise the built-in
`Not_found` exception.**

```ocaml
let smallest xs =
  let rec go xs s =
    match xs with
    | [] -> if s = max_int then raise Not_found else s
    | y::ys -> if y < s && y > 0 then go ys y else go ys s
  in
    go xs max_int;;
```

**Write another function `smallest_or_zero` which uses the `smallest` function
but if `Not_found` is raised, returns zero.**

```ocaml
let smallest_or_zero xs =
  try smallest xs with
    Not_found -> 0;;
```

**Write an `exception` definition and a function which calculates the largest
integer smaller than or equal to the square root of a given integer. If the
argument is negative, the `exception` should be raised.**

```ocaml
exception Negative

let f n =
  if n < 0
    then raise Negative
    else floor (sqrt n);;
```

**Write another function which uses the previous one, but handles the exception,
and simply returns zero when a suitable integer cannot be found.**

```ocaml
let f_catch =
  try f with
    Negative -> 0;;
```

**Comment on the merits and demerits of exceptions as a method for dealing with
exceptional situations, in contrast to returning a special value to indicate an
error (such as `-1` for a function normally returning a positive number).**

> My feeling is throwing exceptions is always less explicit. Returning a special
> value can be defined by types. I don't see demerits of choosing special values
> over exceptions.
