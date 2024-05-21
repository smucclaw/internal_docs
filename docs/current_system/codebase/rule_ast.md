# AST (or CST?) for rules

## Rule

The data type for Rule is defined in [dsl/…/Rule.hs](
https://github.com/smucclaw/dsl/blob/a0ecd7ff35155b1e61c66d17fc511b8e45c60d63/lib/haskell/natural4/src/LS/Rule.hs#L152).

It has the following constructors:

```haskell
-- The most commonly used
Regulative
Constitutive
Hornlike

-- For defining types and their fields
TypeDecl

-- Haven't seen these used much
Scenario
DefNameAlias
DefTypically

-- internal softlink to a rule label (rlabel), e.g. HENCE NextStep
RuleAlias
-- a list of rules + a named section heading
RuleGroup

-- I guess these are like default rules?
RegFulfilled
RegBreach

-- not a rule  ¯\_(ツ)_/¯
NotARule
```

## BoolStruct

There's another library called `anyall`, which defines a datatype called BoolStruct.

```haskell
data BoolStruct lbl a
  = Leaf a
  | All lbl [BoolStruct lbl a] -- and
  | Any lbl [BoolStruct lbl a] --  or
  | Not (BoolStruct lbl a)
  deriving (Eq, Ord, Show, Generic, Hashable, FromJSON, ToJSON, Functor, Foldable, Traversable)
```

`BoolStructR` is a BoolStruct with an optional label (`Maybe Text`) and a `RelationalPredicate`.

## RelationalPredicate

```haskell
data RelationalPredicate =
    RPParamText   ParamText                     -- cloudless blue sky
  | RPMT MultiTerm  -- intended to replace RPParamText. consider TypedMulti?
  | RPConstraint  MultiTerm RPRel MultiTerm     -- eyes IS blue
  | RPBoolStructR MultiTerm RPRel BoolStructR   -- eyes IS (left IS blue AND right IS brown)
  | RPnary RPRel [RelationalPredicate] -- "NEVER GO FULL LISP!" "we went full Lisp".
```

### Types for values

The base for all types is the `MTExpr` type.

```haskell
data MTExpr = MTT Text.Text -- ^ Text string
            | MTI Integer   -- ^ Integer
            | MTF Double     -- ^ Float
            | MTB Bool      -- ^ Boolean

type MultiTerm = [MTExpr]

type TypedMulti = (NonEmpty MTExpr, Maybe TypeSig)

type ParamText = NonEmpty TypedMulti
```

### Types for types

```haskell
data TypeSig = SimpleType ParamType EntityType
             | InlineEnum ParamType ParamText

data ParamType = TOne | TOptional | TList0 | TList1 | TSet0 | TSet1

type EntityType = Text.Text
```

### RPRel

The list of `RPRel`s is as follows.

```haskell
data RPRel =
    RPis | RPhas
  | RPeq | RPlt | RPlte | RPgt | RPgte
  | RPelem | RPnotElem
  | RPnot | RPand | RPor
  | RPmap
  | RPmin | RPmax
  | RPsum | RPproduct | RPminus | RPdivide | RPmodulo
  | RPTC TComparison

data TComparison = TBefore | TAfter | TBy | TOn | TVague
```


### Redundancies in RelationalPredicate

#### ParamText and MultiTerm

```
    RPParamText   ParamText
  | RPMT          MultiTerm
```
Both ParamText and MultiTerm are based on a list of `MTExpr`. ParamText has in addition an optional TypeSig attached to the expressions and two separate NonEmpty lists.

So in translation to Generic MathLang, I just do this

```haskell
case rp of
  RPParamText pt -> expifyBodyRP $ RPMT $ pt2multiterm pt
  RPMT mtes -> expifyMTEsNoMd mtes
```

#### Logical operators from RPRel and BoolStruct

The type `RPRel` contains logical operators `RPnot`, `RPand`, `RPor`. So these things can be in the BoolStructR, or inside the RelationalPredicate.

```haskell
Leaf (RPnary RPnot [rp])
-- should be the same as
Not (Leaf rp)

Leaf (RPnary RPall [rp1, rp2])
-- should be the same as
All lbl [Leaf rp1, Lea rp2]
```

The difference is that the BoolStruct constructors `Any` and `All` include a label (which for BoolStructR is Maybe Text). Maybe there's also some other differences that I'm not aware of?