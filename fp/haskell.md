# Haskell

## String&Text
```
show 3 -- ==> "3"
read $ show 3 :: Int -- ==> 3
```

* [Haskell: Converting Int to String](https://stackoverflow.com/questions/2784271/haskell-converting-int-to-string)

## pattern match
* [Haskell/Pattern matching](https://en.wikibooks.org/wiki/Haskell/Pattern_matching)

## GHCI
* GHCI tries to choose a type signature for your function more strictly than GHC will
```
let func::type; func definition -- make a explicit declaration
```
  * [Haskell No instance for (Num ()) arising from the literal](https://stackoverflow.com/questions/20301976/haskell-no-instance-for-num-arising-from-the-literal)
  * [No instance for (Num[t0]) arising from the literal '2' (Haskell)](https://stackoverflow.com/questions/19086904/no-instance-for-numt0-arising-from-the-literal-2-haskell)
