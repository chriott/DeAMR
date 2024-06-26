# DeAMR 1.0

These are the annotation guidelines for DeAMR (German AMR), which are build in alignment with the official [AMR guidelines](https://github.com/amrisi/amr-guidelines/blob/master/amr.md). The goal is to extend and specify the existing AMR framework to sufficiently map German linguistic phenomena into AMR form.


**Table of Contents**
- [Introduction](#introduction)
  - [Abstract Meaning Representation](#abstract-meaning-representation)
  - [Verb Senses](#verb-senses)
- [Annotation Guidelines](#annotation-guidelines)
  - [Adjectives/adverbs evoking a verb frame](#adjectivesadverbs-evoking-a-verb-frame)
  - [Compounds](#compounds)   
  - [Coordination and Clausal connectives](#coordination-and-clausal-connectives)
  - [Degree](#degree)
  - [Formal pronouns](#formal-pronouns)
  - [Modality](#modality)
  - [Modal particles](#modal-particles)
  - [Nominative case for nouns, pronouns and adjectives](#nominative-case-for-nouns-pronouns-and-adjectives)
  - [Special dashed entities / relations](#special-dashed-entities-and-relations)
- [Limitations](#limitations)
- [References](#references)


# Introduction 

## Abstract Meaning Representation 


AMR is a semantic formalism that captures information on "who is doing what to whom" in a sentence. DeAMR is its German adaptation. AMR (and hence DeAMR) can be visualized as a rooted, directed, acyclic graph. The edges are relations and each node in the graph has a variable labeled with concepts:

<p align="center">
<img src="https://i.ibb.co/djVMtR8/Bildschirmfoto-2022-10-20-um-21-30-43.png" width=50% height=50%>
</p>

AMR and DeAMR use PENMAN-notation, which is a way of representing a graph in a simple, tree-like form:

```lisp
(c1 / zeichnen-01
    :mode imperative
    :ARG0 (c0 / du)
    :ARG1 (c3 / Schaf)
              :mod (c4 / weiß)
    :ARG2 (c2 / ich))
```
Both of the above notations can be rendered into the German sentence: 

> Zeichne mir ein weißes Schaf!

## Verb Senses 

DeAMR uses the German frame set from the [Universal PropBank](https://universalpropositions.github.io) project and their searchable [German PropBank catalogue](http://alanakbik.github.io/UniversalPropositions_German/index.html).

The Universal German PropBank is still in development. If a German frame for a concept is    missing, e.g. *zufrieden*, we have to: 

1. Use the German concept with -00 as sense numbers and find a fitting original PropBank frame to align the argument structure, e.g. for *zufrieden* translates to `zufrieden-00` and the frame structure of [content-01](https://verbs.colorado.edu/propbank/framesets-english-aliases/content.html). A list of the original PropBank frames can be found [here](https://propbank.github.io/v3.4.0/frames/). For functional roles see [here](https://www.isi.edu/~ulf/amr/lib/roles.html).

2. Sometimes, a missing German frame can be replaced with a similar existing German frame. If so, this variant should always be preferred to creating a new frame. For example: 

```lisp
(s / stammen-01
    :ARG1 (d / du
              :mod (m / Mann
                       :degree (k / kleine)))
    :ARG2 (a / amr-unknown))
```
> Wo kommst du her, kleiner Mann?

"Herkommen" does not exist in the German PropBank. `stammen-01`, "to originate from" holds a compatible semantic meaning, so it is used as an alterantive.

# Annotation Guidelines 

## Adjectives/Adverbs evoking a verb frame 

One "slogan" of AMR is to prefer a verb frame whenever it is possible. When annotating adjectives/adverbs in DeAMR, always try to find a fitting verb frame, if there exists one.

```lisp
(e / entgegnen-01
    :ARG0 (p / Prinz
              :mod (k / kleine))
    :ARG1 (e2 / ermöglichen-01
              :polarity -
              :manner (a / anders)
              :ARG0 p)
    :manner (v / verwirren-01
                :mod (g / ganz)))
```
> "Ich kann aber nicht anders", entgegnete der kleine Prinz ganz verwirrt.

Here, the adverb *verwirrt* evokes the verb frame `verwirren-01` and therefore should be used when annotating. 

## Compounds

German is very productive in creating compound words by combining different word classes. To ensure consistent annotation of compounds, follow this heuristic:

<p align="center">
<img src="https://i.ibb.co/tPJnVJ1/Bildschirm-foto-2023-03-14-um-19-05-09.png" width=60% height=60%>
</p>

I. Look up the compound word in the German PropBank.
```

1. If it exists, use it.
2. If it does not, continue below.

```

II. Evaluate if the compound is lexicalized or productive.

```

If productive, lift the semantic head to the top node and find a fitting semantic role for the modifier component

1. If productive, lift the semantic head up to the top node of the compound subgraph and try to find a fitting semantic role for the modifier component (if there is no adequate semantic role use :mod)
2. If not productive, use the word as it is (without numbers).


```

The following examples should provide an intuition:

```lisp
(ä / ähneln-01
   :ARG1 (a / ausbrechen-02
             :ARG1 (v / Vulkan))
   :ARG2 (f / Feuer
             :location (k / Kamin)))
```
> Vulkanausbrüche sind wie Kaminfeuer

`Kaminfeuer` is not lexicalized. `Feuer` occurs in a set of different compounds (e.g. Artilleriefeuer, Lagerfeuer, Grillfeuer) and therefore seems to be productive. According to the guidelines, we lift the semantic head `Feuer` up and match it with a fitting semantic role `:location` for the modifier `Kamin`.

## Coordination and Clausal connectives 

| Example ENG/DE                                         | AMR                        | DeAMR                      |
|--------------------------------------------------------|----------------------------|----------------------------|
 | and/und                                                | and                        | und                        |
 | or/oder                                                | or                         | oder                       |
 | but/aber                                               | `:contrast-01`             | `:contrast-01`             |
 | because/weil; due to/wegen; on account of/aufgrund von | `:cause-01`                | `:cause-01`                |
 | (in order) to/damit; so (that)/sodass                  | `:purpose`                 |  `:purpose`                |
 | if/wenn                                                | `:condition`               |  `:condition`              |
 | unless/außer                                           |  `:condition`, `:polarity` |  `:condition`, `:polarity` |
 | altough/obwohl; despite/trotz                          | `:concession`              |  `:concession`             |

## Degree

Comparatives and superlatives are represented in DeAMR almost the same way as in AMR. Use the same frame `have-degree-91` but match the German attributes and the degree itself.

```
Have-degree-91
Arg1: domain, entity characterized by attribute (e.g. Hund)
Arg2: attribute (e.g. klein)
Arg3: degree itself (e.g. mehr, weniger, gleich, am-meisten, am-wenigsten, genug, mal)
Arg4: compared-to (e.g. (wie die) Katze)
Arg5: superlative: reference to superset
Arg6: reference, threshold of sufficiency (e.g. (klein genug) um im Auto zu sitzen)
```
Example:
```lisp
(c2 / have-degree-91
    :ARG1 (c1 / richten-01
              :ARG0 (c4 / sich)
              :ARG1 c4)
    :ARG2 (c3 / schwer)
    :ARG3 (c6 / viel)
    :ARG4 (c0 / urteilen-01
              :ARG1 (c5 / andere)))
```
> Es ist viel schwerer, über sich selbst zu richten, als über andere zu urteilen.

## Formal pronouns

The formal use of `du` and `ìhr` in German is annotated respectively with `:polite +`

Example:
```lisp
(h / haben-01 :mode expressive 
    :ARG0 (d / du :polite +)
    :ARG1 (h2 / Hut
              :mod (m / merkwürdige)))
```
> Sie haben einen merkwürdigen Hut!

## Modality 

AMR represents syntactic modals with concepts like `possible-01`, `likely-01`, `obligate-01`, `permit-01`, `recommend-01`, `prefer-01`, etc. DeAMR tries to capture Modality in the same way using equivalent German frames:

<p align="center">
<img src="https://i.ibb.co/jb8YWxw/Bildschirm-foto-2023-03-14-um-19-04-20.png" width=60% height=60%>
</p>

| English modal verb | PropBank                   | German modal verb   | German PropBank               | Example                                   |
|--------------------|----------------------------|---------------------|-------------------------------|-------------------------------------------|
| `may`              | `permit-01`, `possible-01` | `dürfen`            | `erlauben-01`, `befähigen-01` | “They may go” / "Sie dürfen gehen"        |
| `can`              | `possible-01`              | `können`            | `ermöglichen-01`, `können-01` | “He can swim” / "Er kann schwimmen"       |
| `should`           | `recommend-01`             | `sollen`            | `empfehlen-01`                | “They should come” / "Sie sollten kommen" |
| `must`             | `obligate-01`              | `müssen`            | `verpflichten-01`             | “He must read” / "Er muss lesen"          |
| `want`             | `prefer-01`                | `wollen`, `möchten` | `bevorzugen-01`               | “He wants to eat” / "Er will essen"       |

```lisp
(c1 / erlauben-01
     :ARG1 (c0 / gehen-01
                :ARG1 (c2 / Junge)))
```
> Der Junge darf gehen.

```lisp
(c1 / ermöglichen-01
     :ARG1 (c0 / schwimmen-01
                :ARG1 (c2 / er)))
```
> Der Junge kann schwimmen.

```lisp
(c1 / empfehlen-01
     :ARG1 (c0 / kommen-01
                :ARG1 (c2 / sie)))
```
> Sie sollten kommen.

```lisp
(c1 / verpflichten-01
     :ARG1 (c0 / lesen-01
                :ARG1 (c2 / er)))
```
> Er muss lesen.

```lisp
(c1 / bevorzugen-01
     :ARG1 (c0 / essen-01
                :ARG1 (c2 / er)))
```
> Er will essen.
> 
> Er möchte essen.

## Modal particles 

German has a large set of different particles. The subset of modal particles is annotated to capture the semantics of the - sometimes convoluted - interaction between the particle itself and the grammatical mood. Here is a table that presents various examples and their corresponding annotations:


| Modal particle                                                                    | Context                                                 | Annotation                     |
|-----------------------------------------------------------------------------------|---------------------------------------------------------|--------------------------------|
| `doch`, `halt`, `ja`, `eben`, `wohl`                                              | "Man weiß halt nie.", "Das ist doch falsch."            | `:mode conclusive`, `:mode resigning`             |
| `doch`, `aber`, `wohl`, `eben`, `halt`, `ja`                                      | "Ich habe schon nachgesehen.", "Das ist schon richtig." | `:mode confirming`             |
| `aber`, `doch`, `schon`, `ja`, `auch`                                             | "Du bist aber schnell!"                                 | `:mode surprised`              |
| `aber`, `doch`, `schon`, `ja`, `auch`                                             | "Das machst du aber gut!"                               | `:mode sarcasm`, `:mode irony` |
| `eigentlich`, `doch`, `denn`, `bloß`, `auch`, `etwa`                              | "Kannst du auch schimmen?"                              | `:mode confirmation-seeking`   |
| `auch`, `bloß`, `doch`, `eben`, `einfach`, `halt`, `mal`, `nur`, `schon`, `ruhig` | "Mach bloß das Fenster zu!"                             | `:mode imperative`             |

```lisp
(c1 / zeichnen-01
    :mode resigning
    :ARG0 (c0 / du)
    :ARG1 (c3 / Schaf)
    :ARG2 (c2 / ich))
```
> Zeichne mir halt ein Schaf.
>
> Zeichne mir doch ein Schaf.

```lisp
(c1 / zumachen-01
    :mode confirmation-seeking
    :ARG0 (c0 / du)
    :ARG1 (c3 / Fenster))
```
> Mach bloß das Fenster zu!
> 
> Mach doch das Fenster zu!

```lisp
(c1 / regnen-01
    :mode conclusive
    :ARG0 (c0 / es))
```
> Es regnet wohl.
> 
> Es wird wohl regnen.

```lisp
(c1 / anfangen-01
    :mode confirming
    :ARG0 (c0 / ich))
```
> Ich habe schon angefangen.
>
> Ich fange schon an.

## Nominative case for nouns, pronouns and adjectives

To ensure consistency, DeAMR uses a labeling convention for pronouns, adverbs, and adjectives.
Specifically, DeAMR demands the nominative case for labeling these elements concerning the word they agree with in the sentence. 

```lisp
(u / überwinden-01
    :mode imperative
    :ARG1 (s / Schweinehund)
             :poss (d / du)}         
             :mod (i / innere))     NOT: (i / inneren)
```
> Überwinde deinen inneren Schweinehund!

## Special dashed entities and relations 

DeAMR aims to be consistent with the original AMR guidelines to ensure easy comparison and integration. Therefore, in agreement to other non-English AMR corpora, it maintains the role labels (e.g. `:ARGX`,
`:location`, `:manner`), AMR-specific framesets (e.g. `be-located-at-91`, `have-degree-91`) and canonical entity types
(e.g. `government-organization`, `political-party`, `person`, `thing`) in English.

For an overview of functional roles and entity types see [here](https://www.isi.edu/~ulf/amr/lib/roles.html).

# References 

[Abstract Meaning Representation (AMR) 1.2.6 Specification](https://github.com/amrisi/amr-guidelines/blob/master/amr.md)

Nathan Schneider, Tim O'Gorman and Jeffrey Flanigang. [AMR Tutorial](https://github.com/nschneid/amr-tutorial/tree/master/slides) presented at NAACL 2015.
