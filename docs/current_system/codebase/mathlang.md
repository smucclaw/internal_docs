# MathLang

Hornlikes from [Rule](./rule_ast.md) can be transformed into MathLang expressions. AFAIK there's no attempt to transform other rule types (regulative, constitutive, …).

## Types

The core of MathLang is the following `Expr` type (reordered here, but all constructors are shown).

```haskell
type ExprLabel = Maybe String
data Expr a =
-- I think these are the most relevant constructors
      Val      ExprLabel a                            -- ^ simple value
    | MathBin  ExprLabel MathBinOp (Expr a) (Expr a)  -- ^ binary arithmetic operation
    | MathVar            String                       -- ^ variable reference
    | MathSet            String    (Expr a)           -- ^ variable assignment
    | MathITE  ExprLabel  (Pred a) (Expr a) (Expr a)  -- ^ if-then-else
    | ListFold ExprLabel SomeFold (ExprList a)        -- ^ fold a list of expressions into a single expr value

-- These I find less important / could probably be restructured?
    | Parens   ExprLabel           (Expr a)           -- ^ parentheses for grouping (??? why is this needed)
    | MathMax  ExprLabel           (Expr a) (Expr a)  -- ^ max of two expressions (ListFold covers this)
    | MathMin  ExprLabel           (Expr a) (Expr a)  -- ^ min of two expressions       "
    | MathApp  ExprLabel String [String] (Expr a)     -- ^ kind of a hack to allow for function to call another function etc. This constructor is removed in final result and the functions are inlined.
    | Undefined ExprLabel -- ^ looks like a quick workaround after realizing there should be a way to recover from failure?

```

There's also similar structure for predicates.

```haskell
-- | conditional predicates: things that evaluate to a boolean
data Pred a
  = PredVal  ExprLabel Bool
  | PredNot  ExprLabel (Pred a)                       -- ^ boolean not
  | PredComp ExprLabel Comp (Expr a) (Expr a)         -- ^ Ord comparisions: x < y
  | PredBin  ExprLabel PredBinOp (Pred a) (Pred a)    -- ^ predicate and / or / eq / ne
  | PredVar  String                                   -- ^ boolean variable retrieval
  | PredSet  String (Pred a)                          -- ^ boolean variable assignment
  | PredITE  ExprLabel (Pred a) (Pred a) (Pred a)     -- ^ if then else, booleans
  | PredFold ExprLabel AndOr (PredList a)             -- ^ and / or a list
```

Both expressions and predicates have list versions. PredList is just a type alias, ExprList has constructors for maps, filters etc. There's `MathSection` for partially applying binary functions, for the purpose of mapping over `ExprList`.

```haskell
type PredList a = [Pred a]

-- | We can filter, map, and mapIf over lists of expressions. Here, @a@ is pretty much always a @Double@.
data ExprList a
  = MathList  ExprLabel [Expr a]                                    -- ^ a basic list of `Expr` expressions
  | ListMap   ExprLabel (MathSection a)               (ExprList a) -- ^ apply the function to everything
  | ListFilt  ExprLabel                 (Expr a) Comp (ExprList a) -- ^ eliminate the unwanted elements
  | ListMapIf ExprLabel (MathSection a) (Expr a) Comp (ExprList a) -- ^ leaving the unwanted elements unchanged
  | ListConcat ExprLabel [ExprList a] -- ^ [[a]] -> [a]
  | ListITE    ExprLabel (Pred a) (ExprList a) (ExprList a)        -- ^ if-then-else for expr lists

data MathSection a
  = Id
  | MathSection MathBinOp (Expr a)

data MathBinOp = Plus | Minus | Times | Divide | Modulo
```

## Pipeline

1. Spreadsheet gets parsed into a Rule
2. Rule (only Hornlike) gets transformed into [Generic MathLang](./generic_mathlang.md)
3. Generic MathLang is transformed into MathLang.
