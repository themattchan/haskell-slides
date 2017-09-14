# lens

- a lens doesn't just have to be an accessor -- can lift predicates to lenses

```hs

data Element = Element
  { atomicNumber :: Int, massNumber :: Double }

isCobalt (Element a _) = a == 27

si = Element 14 28.08
co = Element 27 58.93
he = Element 2   4.00
elements = [si,co,he]

xxxx = do
  co ^. to isCobalt  @?= True
  si ^. to isCobalt  @?= False

  -- contains a match
  orOf (traverse . to isCobalt) elements @?= True

  -- every element matches
  andOf (traverse . to isCobalt) elements @?= False

  -- count the number of occurrences
  lengthOf (traverse . filtered isCobalt) elements @?= 1
```

- note that the `{and,or}Of` take `Getters` where the target is a `Boolean`, so
  you simply lift the predicate to a `Getter` with `to`
- with `{toList, length}Of` you have to make it a filter predicate with `filtered`
