# Reading the Contents of a File in Reason Native

If you want to read in the contents of a file solely using the built-in
`Pervasives` module:

```reason
let readFile = path => {
  let ch = open_in(path);
  let s = really_input_string(ch, in_channel_length(ch));
  close_in(ch);
  s;
};

```

If you prefer OCaml syntax:

```ocaml
let read_file path =
    let ch = open_in filename in
    let s = really_input_string ch (in_channel_length ch) in
    close_in ch;
    s
```

TODO: Explanation of code

Source:
https://stackoverflow.com/questions/53839695/how-do-i-read-the-entire-content-of-a-given-file-into-a-string/53840784#53840784
