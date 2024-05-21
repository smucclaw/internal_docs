# Explainable

## Types

```haskell
type ExplainableIO  r st a = RWST         (HistoryPath,r) [String] st IO (a,XP)
type Explainable    r st a = ExplainableIO r st a
```

## Evaluation

TODO