<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
This document specifies the core ESTree AST node types that support the ES5 grammar.

- [Node objects](#node-objects)
- [Identifier](#identifier)
- [Programs](#programs)
- [Functions](#functions)
- [Statements](#statements)
  - [ExpressionStatement](#expressionstatement)
  - [BlockStatement](#blockstatement)
  - [Control flow](#control-flow)
    - [ReturnStatement](#returnstatement)
- [Expressions](#expressions)
  - [FunctionExpression](#functionexpression)
  - [CallExpression](#callexpression)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Node objects

ESTree AST nodes are represented as `Node` objects, which may have any prototype inheritance but which implement the following interface:

```js
interface Node {
    type: string;
    loc: SourceLocation | null;
}
```

The `type` field is a string representing the AST variant type. Each subtype of `Node` is documented below with the specific string of its `type` field. You can use this field to determine which interface a node implements.

The `loc` field represents the source location information of the node. If the node contains no information about the source location, the field is `null`; otherwise it is an object consisting of a start position (the position of the first character of the parsed source region) and an end position (the position of the first character after the parsed source region):

```js
interface SourceLocation {
    source: string | null;
    start: Position;
    end: Position;
}
```

Each `Position` object consists of a `line` number (1-indexed) and a `column` number (0-indexed):

```js
interface Position {
    line: number; // >= 1
    column: number; // >= 0
}
```

# Identifier

```js
interface Identifier <: Expression {
    type: "Identifier";
    name: string;
}
```

An identifier. Note that an identifier may be an expression or a destructuring pattern.

# Programs

```js
interface Program <: Node {
    type: "Program";
    body: [ Statement ];
}
```

A complete program source tree.

# Functions

```js
interface Function <: Node {
    id: Identifier | null;
    params: [ Identifier ];
    body: BlockStatement;
}
```

A function [declaration](#functiondeclaration) or [expression](#functionexpression).

# Statements

```js
interface Statement <: Node { }
```

Any statement.

## ExpressionStatement

```js
interface ExpressionStatement <: Statement {
    type: "ExpressionStatement";
    expression: Expression;
}
```

An expression statement, i.e., a statement consisting of a single expression.

## BlockStatement

```js
interface BlockStatement <: Statement {
    type: "BlockStatement";
    body: [ Statement ];
}
```

A block statement, i.e., a sequence of statements surrounded by braces.

## Control flow

### ReturnStatement

```js
interface ReturnStatement <: Statement {
    type: "ReturnStatement";
    argument: Expression | null;
}
```

A `return` statement.

# Expressions

```js
interface Expression <: Node { }
```

Any expression node. Since the left-hand side of an assignment may be any expression in general, an expression can also be a pattern.

## FunctionExpression

```js
interface FunctionExpression <: Function, Expression {
    type: "FunctionExpression";
}
```

A `function` expression.

## CallExpression

```js
interface CallExpression <: Expression {
    type: "CallExpression";
    callee: Expression;
    arguments: [ Expression ];
}
```

A function or method call expression.
