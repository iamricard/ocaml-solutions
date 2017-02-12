**Write a function which multiplies a given number by ten. What is its type?**

```ocaml
(* int -> int *)
let times_ten x =
  x * 10;;
```

**Write a function which returns true if both of its arguments are non-zero,
and false otherwise. What is the type of your function?**

```ocaml
(* int -> int -> bool *)
let non_zeros x y =
  x <> 0 && y <> 0;;
```

**Write a recursive function which, given a number `n`, calculates the sum
`1 + 2 + 3 + ... + n`. What is its type?**

```ocaml
(* int -> int *)
let rec sum_up_to n =
  if n = 0 then n else n + sum_up_to (n - 1);;
```

**Write a function power x n which raises x to the power n. Give its type.**

```ocaml
(* int -> int -> int *)
let rec power x n =
  if n = 0 then 1 else
    if n = 1 then x else
      x * power x (n - 1);;
```

**Write a function `isconsonant` which, given a lower-case character in the
range `'a'...'z'`, determines if it is a consonant.**

```ocaml
(* char -> bool *)
let is_consonant c =
  c <> 'a' && c <> 'e' && c <> 'i' && c <> 'o' && c <> 'u'
```

**What is the result of the expression `let x = 1 in let x = 2 in x + x`?**

```ocaml
4;; (* because shadowing *)
```

**Can you suggest a way of preventing the non-termination of the factorial
function in the case of a zero or negative argument?**

```ocaml
(* we could return zero in those cases *)
let rec factorial x =
  if x <= 0 then 0 else
    if x = 1 then 1 else
      x * factorial (x - 1)
```
