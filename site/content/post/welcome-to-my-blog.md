---
title: Welcome to My Blog
date: 2018-04-13T18:14:23.386Z
description: This is my blog.
---
This is a test post.

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

```c#
public void Main()
{
    var a = 1;
}
```
