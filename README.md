# d20

[![PyPI version shields.io](https://img.shields.io/pypi/v/d20.svg)](https://pypi.python.org/pypi/d20/)
[![PyPI license](https://img.shields.io/pypi/l/d20.svg)](https://pypi.python.org/pypi/d20/)
[![PyPI pyversions](https://img.shields.io/pypi/pyversions/d20.svg)](https://pypi.python.org/pypi/d20/)
![](https://github.com/avrae/d20/workflows/Test%20Package/badge.svg)



A fast, powerful, and extensible dice engine for D&D, d20 systems, and any other system that needs dice!

## Key Features
- Quick to start - just use `d20.roll()`!
- Optimized for speed and memory efficiency
- Highly extensible API for custom behaviour and dice stringification
- Built-in execution limits against malicious dice expressions
- Tree-based dice representation for easy traversal 

## Installing
**Requires Python 3.6+**.

d20 is built on top of the amazing [Lark](https://github.com/lark-parser/lark) parser, and has it as its only dependency!

```
python3 -m pip install -U d20
```

## Quickstart

```python
>>> import d20
>>> result = d20.roll("1d20+5")
>>> str(result)
'1d20 (10) + 5 = `15`'
>>> result.total
15
>>> result.crit
<CritType.NORMAL: 0>
>>> str(result.ast)
'1d20 + 5'
```

## Dice Syntax
This is the grammar supported by the dice parser, roughly ordered in how tightly the grammar binds.

### Numbers
These are the atoms used at the base of the syntax tree.

| Name    | Syntax            | Description       | Examples           |
|---------|-------------------|-------------------|--------------------|
| literal | `INT`, `DECIMAL`  | A literal number. | `1`, `0.5`, `3.14` |
| dice    | `INT? "d" INT`    | A set of die.     | `d20`, `3d6`       |
| set     | `"(" (num ("," num)* ","?)? ")"` | A set of expressions. | `()`, `(2,)`, `(1, 3+3, 1d20)` |

Note that `(3d6)` is equivalent to `3d6`, but `(3d6,)` is the set containing the one element `3d6`.

### Set Operations
These operations can be performed on dice and sets.

#### Grammar
| Name    | Syntax            | Description       | Examples           |
|---------|-------------------|-------------------|--------------------|
| set_op  | `operation selector` | An operation on a set (see below). | `kh3`, `ro<3` |
| selector | `seltype INT` | A selection on a set (see below). | `3`, `h1`, `>2` |

#### Operators
Operators are always followed by a selector, and operate on the items in the set that match the selector.

| Syntax | Name | Description |
|---|---|---|
| k | keep | Keeps all matched values. |
| p | drop | Drops all matched values. |
| rr | reroll | Rerolls all matched values until none match. (Dice only) |
| ro | reroll once | Rerolls all matched values once. (Dice only) |
| ra | reroll and add | Rerolls all matched values once, keeping the original roll. (Dice only) |
| e | explode on | Rolls another die for each matched value. (Dice only) |
| mi | minimum | Sets the minimum value of each die. (Dice only) |
| ma | maximum | Sets the maximum value of each die. (Dice only) |

#### Selectors
Selectors select from the remaining kept values in a set.

| Syntax | Name | Description |
|---|---|---|
| X | literal | All values in this set that are literally this value. |
| hX | highest X | The highest X values in the set. |
| lX | lowest X | The lowest X values in the set. |
| \>X | greater than X | All values in this set greater than X. |
| <X | less than X | All values in this set less than X. |

### Unary Operations
| Syntax | Name | Description |
|---|---|---|
| +X | positive | Does nothing. |
| -X | negative | The negative value of X. |

### Binary Operations
| Syntax | Name |
|---|---|
| X * Y | multiplication |
| X / Y | division |
| X // Y | int division |
| X % Y | modulo |
| X + Y | addition |
| X - Y | subtraction |
| X == Y | equality |
| X >= Y | greater/equal |
| X <= Y | less/equal |
| X > Y | greater than |
| X < Y | less than |
| X != Y | inequality |

## Documentation

TODO