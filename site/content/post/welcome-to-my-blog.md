---
title: Welcome to my blog!
date: 2018-04-13T19:15:04.000Z
tags:
  - test
  - hello
image: ''
---
```f#
let tryMap f xs =
    let rec loop ys = function
        | [] -> Some (List.rev ys)
        | x::xs ->
            match f x with
            | None -> None
            | Some y -> loop (y::ys) xs
    loop [] xs
```
