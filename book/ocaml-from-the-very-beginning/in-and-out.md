**Write a function to print a list of integers to the screen in the same format
OCaml uses – i.e. with square brackets and semicolons.**

```ocaml
let print_array ints =
  let rec iterate xs =
    match xs with
    | [] -> ()
    | [i] -> print_string (string_of_int i)
    | i :: rest ->
      print_string (string_of_int i);
      print_string "; ";
      iterate rest
  in
    print_char '[';
    iterate ints;
    print_char ']';
;;
```

**Write a function to read three integers from the user, and return them as a
tuple. What exceptions could be raised in the process? Handle them
appropriately.**

```ocaml
let rec read_to_tuple () =
  try
    let x = read_int () in
      let y = read_int () in
        let z = read_int () in
          (x, y, z)
  with
    Failure "int_of_string" ->
      print_newline ();
      read_to_tuple ()
```

**In our `read_dict` function, we waited for the user to type `0` to indicate no
more data. This is clumsy. Implement a new `read_dict` function with a nicer
system. Be careful to deal with possible exceptions which may be raised.**

```ocaml
let rec collect_entries count =
  match count with
  | 0 -> []
  | c ->
    try
      print_string "Key:\n> ";
      let key = read_int () in
        print_string "Value:\n> ";
        let value = read_line () in
          (key, value) :: collect_entries (c - 1)
    with
      Failure "int_of_string" ->
        print_string "Keys must be a valid integer.";
        print_newline ();
        collect_entries c
;;

let rec read_dict () =
  try
    print_string "How many entries will the dictionary have?\n> ";
    let count = read_int () in
      collect_entries count
  with
    Failure "int_of_string" ->
      print_string "The amount of entries has to be a valid integer.";
      print_newline ();
      read_dict()
;;
```

**Write a function which, given a number `x`, prints the `x`-times table to a
given filename. For example, `table "table.txt" 5` should produce a file
`table.txt` containing the following (see book):**

```ocaml
let row x y =
  let rec go r x' y' =
    match x' with
    | 0 -> r
    | x' -> go (x' * y :: r) (x' - 1) y
  in
    go [] x y
;;

let table_for x =
  let rec go x' row_number =
    if row_number > x' then []
    else row x' row_number :: go x' (row_number + 1)
  in
    go x 1
;;

let rec output_row ch r =
  match r with
  | [] -> ()
  | col :: rest ->
    output_string ch (string_of_int col);
    output_string ch "\t";
    output_row ch rest
;;

let rec output_table ch t =
  match t with
  | [] -> ()
  | row :: rest ->
    output_row ch row;
    output_string ch "\n";
    output_table ch rest
;;

let table filename n =
  let ch = open_out filename in
    output_table ch (table_for n);
    close_out ch
;;
```

**Write a function to count the number of lines in a given file.**

```ocaml
let rec count_lines file =
  try
    let _ = input_line file in
      1 + count_lines file
  with
    End_of_file -> 0
;;

let lines filename =
  let file = open_in filename in
    count_lines file
;;
```

**Write a function `copy_file` of type `string → string → unit` which copies a
file line by line. For example, `copy_file "a.txt" "b.txt"` should produce a
file `b.txt` identical to `a.txt`. Make sure you deal with the case where the
file `a.txt` cannot be found, or where `b.txt` cannot be created or filled.**

```ocaml
let rec copy_contents source target =
  try
    output_string target (input_line source);
    output_string target "\n";
    copy_contents source target
  with
    End_of_file ->
      close_in source;
      close_out target
;;

exception FileError;;

let copy source target =
  try
    let source_ch = open_in source in
      let target_ch = open_out target in
        copy_contents source_ch target_ch
  with
    _ -> raise FileError
;;
```
