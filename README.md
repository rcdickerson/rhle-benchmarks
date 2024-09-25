# RHLE Benchmarks

This repository contains benchmark verification tasks which require relational
Hoare logic with existentials (RHLE). These benchmarks are part of the
evaluation of [ORHLE](https://github.com/rcdickerson/orhle) which appeared in
[RHLE: Modular Deductive Verification of Relational ∀∃
Properties](https://arxiv.org/abs/2002.02904).

## Citing

If you use these benchmarks, please cite the original [RHLE
paper](https://robd.io/papers/rhle.pdf):

```
@inproceedings{dickerson2022rhle,
  title={{RHLE}: modular deductive verification of relational $\forall\exists$ properties},
  author={Dickerson, Robert and Ye, Qianchuan and Zhang, Michael K and Delaware, Benjamin},
  booktitle={Asian Symposium on Programming Languages and Systems},
  pages={67--87},
  year={2022},
  organization={Springer}
}
```

## Benchmark Language and File Format

Benchmarks are written using the FunIMP language described in the [RHLE
paper](https://arxiv.org/abs/2002.02904). Benchmark files are fomratted
as follows:

```
expected: <expect>;

forall: <execName>*
exists: <execName>*

pre: <smtlib2>
post: <smtlib2>

aspecs: <aspec>*
especs: <espec>*

<funDef>*
```

where

+ `<expect> ::= valid | invalid` indicates whether the file is
  expected to verify. The `expected` tag is optional and entirely for
  bookkeeping purposes.
+ `<execName> ::= <string>([<string>])?` is the name of a FunIMP
  function defined later in the file optionally annotated with an
  execution identifier. The execution identifier distinguishes
  between different executions of the same function. For example,
  a single function `foo` appearing on both the ∀ and ∃ sides
  of a RHLE triple might be given as:
  ```
  forall: foo[A]
  exists: foo[E]
  ```
+ `<smtlib2>` represents a specification given as an SMTLib2 string.
  Variables inside functions are referenced by using `!` as a
  separator; for example, if function `foo` has a variable `x`, SMTLib
  specifications can refer to the value as `foo!x`. If a function
  as multiple executions, the execution ID should appear after
  the function name separated by `!`; for example, value `x`
  in `foo[A]` should be referred to as `foo!A!x`.
+ `<aspec> ::= <string>(<params>) { pre: <smtlib2>; post: <smtlib2>;
  }` gives a universal specification for some function. The `pre` and
  `post` specifications may reference any program state and the
  function parameters. Additionally, the postcondition can reference
  the special variable `ret!` to refer to the function's return value.
+ `<espec> ::= <string>(<params>) { choiceVars: <string>*; pre:
  <smtlib2>; post: <smtlib2>; }` gives an existential specification
  for some function, where `choiceVars` is a list of choice variable
  names that may be referenced in the `pre` and `post` conditions.
+ `<funDef> ::= fun <string>(<params>) { <FunIMP> }` is a function
  definition. `<FunIMP>` is FunIMP syntax as given in Figure 2 of
  the paper. At the time of writing, loops must be decorated
  with invariants given as `@inv { <smtlib2> }`. Loops in existential
  executions must also be decorated with variants given as `@var {
  <smtlib2> }`
+ `<params>` is a comma-separated list of strings giving function
  parameter names.
