# Gnoll Code Snippets

The following code snippets are just blocks of code I found or created that I wanted to remember. This page is primarily used as a reference for me but if you find any code useful then I am glad. :)

## ChessEOS Snippets

### Chess Board Array [['a1','a2'...],['b1','b2',...]...]

```
// multi-dim array
[...Array(8)].map((y, i) => [...'abcdefgh'].map(x => x + (8 - i)))

// single array
[...Array(8)].map((y, i) => [...'abcdefgh'].map(x => x + (8 - i))).flat()
```
