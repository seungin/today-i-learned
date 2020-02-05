# cmake-commands(7) : string

문자열을 처리한다.

## Examples

`FIND` substring을 찾는다.

```cmake
set(string "this is a string")

set(substring "is")
string(FIND ${string} ${substring} out_var)
message("out_var: ${out_var}")

set(substring "unknown")
string(FIND ${string} ${substring} out_var)
message("out_var: ${out_var}")
```

```txt
out_var: 2
out_var: -1
```

`REPLACE` 문자열을 교체한다.

```cmake
set(path "C:/Program Files/CMake/bin")
string(REPLACE "/" "\\" path ${path})
message("path: ${path}")
```

```txt
path: C:\Program Files\CMake\bin
```
