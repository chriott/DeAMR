# DeAMR
Annotation guidelines DeAMR (German AMR)

(WIP in vsc)

Reference CAMR+ Spanish AMR

**Table of Contents**
- [Introduction](#introduction)
   - [Abstraction away from English](#Abstraction-away-from-English)
   - [Verb Senses](#verb-senses)
- [Annotation Guidelines](#annotation-guidelines)
   - [Modality](#modality)
   - [Modal particles](#modal-particles) 

Introduction
====================

Abstraction away from English
----------------------------

```lisp
(m / mean-01
      :ARG1 (a / admire-01)
      :ARG2 (r / regard-01
            :ARG0 (y / you)
            :ARG1 (i / i)
            :ARG2 (m6 / man
                  :ARG1-of (h2 / have-degree-91
                        :ARG2 (h / handsome)
                        :ARG3 (m2 / most)
                        :ARG5 (p2 / planet
                              :mod (t / this)))
                  :ARG1-of (h3 / have-degree-91
                        :ARG2 (w2 / well-09
                              :ARG1 (d / dress-01
                                    :ARG1 m6))
                        :ARG3 (m3 / most)
                        :ARG5 p2)
                  :ARG1-of (h4 / have-degree-91
                        :ARG2 (r2 / rich)
                        :ARG3 (m4 / most)
                        :ARG5 p2)
                  :ARG1-of (h5 / have-degree-91
                        :ARG2 (i2 / intelligent-01
                              :ARG1 m6)
                        :ARG3 (m5 / most)
                        :ARG5 p2))))
```

```lisp
(c1 / zeichnen-01
    :ARG0 (c0 / du)
    :ARG1 (c3 / Schaf)
    :ARG2 (c2 / ich))
```

Verb Senses
-----------
We are using the German verb senses from the Universal PropBank project https://universalpropositions.github.io. (German PropBank catalogue http://alanakbik.github.io/UniversalPropositions_German/index.html) If a sense of a word or a verb itself does not appear in the Universal German PropBank lexicon, we define its sense number in a table of verbs and senses. See below the growing list of verbs and senses which do not appear in Universal German PropBank, with the corresponding English translation.

Annotation Guidelines
====================

Special/Dashed entities/relations + functional relations
--------------------------------------------------------

WIP

For the other special roles in English, such as have-degree-91 and quant-91, we will use these English terms.

For reasons of compatibility (?).

Modality 
--------

Reference: Note the presence of deber and poder in the list of words which appear in AnCora with other senses. Though meanings of deber and poder do appear in AnCora, we establish additional senses to mark modality. These modals take the same :ARG1 structure as do their English modal equivalents– recommend-01 and possible-01, respectively. These modals take the verb senses deber-03 and poder-04.

AMR represents syntactic modals with concepts like possible-01, likely-01, obligate-01, permit-01, recommend-01, prefer-01, etc. Here is a conversion table of DeAMR capturing equivalent modal semantics.

English modal verb     |     PropBank       | German modal verb   | German PropBank   | Example
-----------------------|--------------------|---------------------|-------------------|---------
`may`                  | `permit-01`, `possible-01` (?) | `dürfen`| `erlauben-01`     | “They may go” / "Sie dürfen gehen"
`can`                  | `possible-01`      | `können`            | `ermöglichen-01`(?)  | “He can swim” / "Er kann schwimmen"
`should`               | `recommend-01`     | `sollen`            | `empfehlen-01`    | “They should come” / "Sie sollten kommen"
`must`                 | `obligate-01`      | `müssen`            | `verpflichten-01` | “He must read” / "Er muss lesen"
`want`                 | `prefer-01`        | `wollen`, `möchten` | `bevorzugen-01`   | “He wants to eat” / "Er will essen"


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
> Er will lesen.
> 
> Er möchte essen.


Modal particles
---------------

Modal particle     | Context           | Annotation   
-------------------|-------------------|----------------------------------
`doch`, `halt`, `schon` | Mach doch das Fenster zu! | `:mode emphasis`

```lisp
(c1 / zeichnen-01
    :mode emphasis
    :ARG0 (c0 / du)
    :ARG1 (c3 / Schaf)
    :ARG2 (c2 / ich))
```
> Zeichne mir halt ein Schaf.
>
> Zeichne mir doch ein Schaf.
>
