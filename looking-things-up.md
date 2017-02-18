**Write a function to determine the number of different keys in a dictionary.**

```ocaml
let key_count d =
  length d;;
```

**Define a function `replace` which is like `add`, but raises `Not_found` if the
key is not already there.**

```ocaml
let replace k v d =
  let rec go key value dict =
    match dict with
    | [] -> raise Not_found
    | (key', value')::rest ->
      if key' == key
        then (key, value) :: rest
        else (key', value') :: go key value rest
  in
    go k v d;;
```

**Write a function to build a dictionary from two equal length lists, one
containing keys and another containing values. Raise the exception
`Invalid_argument` if the lists are not of equal length.**

```ocaml
(* This function might create duplicated keys *)
let rec from_pairs (keys, values) =
  match keys, values with
  | [], [] -> []
  | _, [] | [], _ -> raise (Invalid_argument "Lists are not of equal length")
  | k::ks, v::vs -> (k, v) :: from_pairs (ks, vs);;
```

**Now write the inverse function: given a dictionary, return the pair of two
lists – the first containing all the keys, and the second containing all the
values.**

```ocaml
let rec to_pairs d =
  match d with
  | [] -> ([], [])
  | (key, value)::rest ->
    match to_pairs rest with
    | (keys, values) -> (key :: keys, value :: values);;
```

**Define a function to turn any list of pairs into a dictionary. If duplicate
keys are found, the value associated with the first occurrence of the key should
be kept.**

```ocaml
let rec key_exists key dict =
  match dict with
  | [] -> false
  | (key', _) :: rest -> key == key' || key_exists key rest;;

let dict_from_pairs ps =
  let rec go dict pairs =
    match pairs with
    | [] -> dict
    | (key, value) :: rest ->
      if key_exists key dict
        then go dict rest
        else go ((key, value) :: dict) rest
  in
    go [] ps;;
```

**Write the function `union a b` which forms the union of two dictionaries. The
union of two dictionaries is the dictionary containing all the entries in one or
other or both. In the case that a key is contained in both dictionaries, the
value in the first should be preferred.**

```ocaml
let union dict_a dict_b =
  dict_from_pairs (List.concat [dict_a; dict_b]);;
```
