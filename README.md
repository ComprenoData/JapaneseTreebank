This repository contains a Japanese treebank annotated by ABBYY Compreno project.

All original sentences in the treebank come from Tatoeba project https://tatoeba.org/eng/ and are distributed under Creative Commons Attribution License 2.0 (http://creativecommons.org/licenses/by/2.0/fr/).

Annotation is distributed under Creative Commons Attribution Licence 4.0 (https://creativecommons.org/licenses/by/4.0/legalcode).
Copyright © 2017 ABBYY. All rights reserved.




# ABBYY Compreno Annotation Manual

## ABBYY Compreno Syntactic Model

A view from dependency syntax:
* Dependency trees are __projective__: leaves of a subtree form a contiguous string of words.
* Functional elements depend on lexical elements: prepositions attach to nouns, subordinate conjunctions to verbs, auxiliary verbs to lexical verbs.
* Coordination is __not__ a dependency relation: coordination is a special non-tree relation between “sister” nodes, which exists on a level of its own.

A view from constituent syntax:
* Every constituent has a lexical core: e.g. noun phrases are organized around nouns etc. All productions have the form: `XP -> [… X …]`.
* Constituents are “flat”: all complements and modifiers to the core are sisters to each other.
* Syntactic functions and semantic roles of all sub-constituents are explicitly annotated.

## ABBYY Compreno Annotation Syntax

### General principles
* We __do not__ demand all markup decisions to be made at once. Markup can be a slow incremental process.
* The purpose of markup is not to specify the single correct structure, but to set some constraints to the parser.
* However, the combination of our parser, our dictionary and explicit markup can be restrictive enough.

### Annotation mode
* Annotation is “on” if the text starts with a leading hash symbol (`#`).
> _This is raw text._
>
> _<b>#</b>This is annotated text._

* When annotation is “on”, the following 12 symbols have special meaning: `$ : < > [ ] { } | @ " #`. To be interpreted literally, they must be __escaped__ by the hash symbol:
> _This is raw text: example@abbyy.com._
>
> _#This is annotated text#: example#@abbyy.com._

### Constituents
* Constituents are annotated with square brackets.
* Absence of square brackets __does not imply__ absence of a constituent.

Examples (all valid):
> _#[A quick fox] jumped over a lazy dog._
>
> _#[A quick fox] jumped [over a lazy dog]._
>
> _#[[A quick fox] jumped [over a lazy dog]]._
>
> _#[[[A] [quick] fox] jumped [[over] [a] [lazy] dog]._

### Tokenization
Explicit tokenization:
* Tokenization is annotated with curly braces: _{anti}{matter}_ (2 tokens), _{because of}_ (1 token).
* Absence of curly braces generally means that tokenization is unspecified.

Implicit tokenization:
* All markup operators (except `[]` and `{}`) attach to tokens from the left or from the right.
* In absence of explicit tokenization, a token is assumed to be bounded by spaces, punctuation (or other non-alphanumeric characters).

Examples:
> _because of |1|_  == _because {of} |1|_
>
> _antimatter |1|_ == _{antimatter} |1|_

### Special treatment for whitespace
* Whitespace between markup operators and tokens is ignored.

### Token IDs
* A token ID is a number (chosen at random). This number should be unique.
* Token IDs are written in vertical bars after the corresponding tokens:
> _#Colourless|1| green |2| ideas  |42| sleep furiously._

The purpose of annotating token IDs is explained below.

### Dependency
* Dependency relation is annotated on the child. It attaches to the left.
* Three things can go into dependency annotation: 1) parent ID, 2) syntactic function, 3) semantic role.
* Syntactic function is prefixed with dollar sign.

Any order is allowed:
> _#The ($Subject, Agent, 1): farmer killed |1| the duckling._
>
> _#The (1, Agent, Subject): farmer killed |1| the duckling._

Annotation can be partial (just 2 things or 1 thing):
> _#The (Agent, $Subject): farmer killed the duckling._
>
> _#The (1, $Subject): farmer |1| killed the duckling._
>
> _#The (1): farmer killed |1| the duckling._
>
> _#The (Agent): farmer killed the duckling._
>
> _#The ($Subject): farmer killed the duckling._

Brackets are optional if only one thing is annotated:
> _#The 1: farmer killed |1| the duckling._
>
> _#The $Subject: farmer killed the duckling._
>
> _#The Agent: farmer killed the duckling._

* Dependency usually can be reconstructed from constituents annotation. For example, dependency `x -> y` is possible with `[x [y]]` and impossible with `[x [[y] z]]`.

### Anaphora
* Anaphoric relation is annotated on the anaphor (usually a pronoun). It attaches to the right.
> _#John |1| loves himself @1._


