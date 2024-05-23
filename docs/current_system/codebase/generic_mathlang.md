# Generic MathLang

Generic MathLang is meant to be an intermediate step between the [Rule datatype](./rule_ast.md) and [MathLang](./mathlang.md).

Things that get more structured from Rule. A lot of things in Rule are just free text inside a `MTExpr`, but it gets parsed in Generic MathLang.

## Arithmetic expressions

In Rule, a single cell may contain arbitrary expressions. Compare lines 1 and 2:

```L4
SUM,foo,bar,,,,,,, (1)
,,,,,,,,,
foo + bar,,,,,,,,, (2)
```

Version (1) is parsed as an arithmetic expression in Rule, but version (2) is just free text. Generic MathLang parses everything it can, even inside the cells.

## Records

Text like `foo's,bar's,baz` gets parsed into records: `foo.bar.baz`.

## Functions

Rule may define functions as follows (see [example spreadsheet](https://docs.google.com/spreadsheets/d/1cWAb7Ba4HJovQn1PquZzYJjnjKUuhEPhNHzAH4ZfV4I/edit#gid=2100528279) for larger context):

```L4
GIVEN	x			IS A	Number
        y			IS A	Number
DECIDE	x	discounted by		y	IS	x * (1 - y)
```

And we can apply the function (simplified the arguments):

```L4
DECIDE Answer	IS	firstArg	discounted by   secondArg
```

When parsed into rules, we don't have much structure:

```haskell
HC { hHead = RPConstraint
            [ MTT "x", MTT "discounted by", MTT "y" ] RPis
            [ MTT "x * (1 - y)" ]
   , hBody = Nothing}

HC { hHead = RPConstraint
            [ MTT "Answer" ] RPis
            [ MTT "firstArg"
            , MTT "discounted by"
            , MTT "secondArg" ]
   , hBody = Nothing}
```

The definition becomes as follows:

```haskell
( "discounted by",
        (
            [ MkVar "x", MkVar "y" ], MkExp
            { exp = ENumOp
                { numOp = OpMul, nopLeft = MkExp
                    { exp = EVar
                        { var = MkVar "x" }, md = [{- omitted some metadata: line number, type etc. -}]
                    }, nopRight = MkExp
                    { exp = ENumOp
                        { numOp = OpMinus, nopLeft = MkExp
                            { exp = ELit
                                { lit = EInteger 1 }, md = [{- omitted some metadata -}]
                            }, nopRight = MkExp
                            { exp = EVar
                                { var = MkVar "y" }, md = [{- omitted some metadata -}]
                            }
                        }, md = [{- omitted some metadata -}]
                    }
                }, md = [{- omitted some metadata -}]
            }
        )
    )
```

And the application as follows:

```haskell
EVarSet
    { vsetVar = MkExp
        { exp = EVar { var = MkVar "Answer" }, md = [] }, arg = MkExp
        { exp = EApp
            { func = MkExp
                { exp = EApp
                    { func = MkExp
                        { exp = EVar
                            { var = MkVar "discounted by" }, md = [{- omitted some metadata -}]
                        }, appArg = MkExp
                        { exp = EVar
                            { var = MkVar "Step 3" }, md = [{- omitted some metadata -}]
                    }, md = []
                }, appArg = MkExp
                { exp = ERec
                    { fieldName = MkExp
                        { exp = EVar
                            { var = MkVar "risk cap" }, md = [{- omitted some metadata -}]
                        }, recName = MkExp
                        { exp = EVar
                            { var = MkVar "accident" }, md = [{- omitted some metadata -}]
                        }
                    }, md = []
                }
            }, md = []
        }
    }
```

## Types

[](https://github.com/smucclaw/dsl/blob/main/lib/haskell/natural4/src/LS/XPile/MathLang/GenericMathLang/GenericMathLangAST.hs)
