# DeAMR
Annotation guidelines DeAMR (German AMR) try to be in alignment with the official AMR guidelines.

(WIP in vsc)

Reference CAMR+ Spanish AMR

**Table of Contents**
- [Introduction](#introduction)
   - [Abstraction away from English](#Abstraction-away-from-English)
   - [Verb Senses](#verb-senses)
- [Annotation Guidelines](#annotation-guidelines)
   - [Compounds](#compounds)
   - [Degree](#degree)
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

The Universal German PropBank is still in development and incomplete. Hence, if a required German frame for a concept should be missing, e.g. "zufrieden", one workaround could be to use German concept without numbers and find an equivalent English frame to align the argument structure, e.g. for zufrieden use [content-01](https://verbs.colorado.edu/propbank/framesets-english-aliases/content.html).

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

In German, there exists a large set of particles. Modal particles are annotated in a way that captures the semantic of the - sometimes opaque - interaction between the particle itself and the grammatical moods. Here is a table that illuminates different possible contexts and the corresponding annotation:

Modal particle          | Context                                          | Annotation   
------------------------|--------------------------------------------------|----------------------------------
`doch`, `halt`, `schon` | "Man weiß halt nie", "Hör mal zu!"               | `:mode emphasis`
`doch`, `aber`, `wohl`, `eben`, `halt`, `ja` | "Ich habe schon nachgesehen", "Es wird wohl regnen"  | `:mode assertive`
`aber`, `doch`, `schon` | "Du bist aber schnell!"                          | `:mode surprised`
`aber`                  | "Das machst du aber gut!"                        | `:mode sarcasm`, `:mode irony`



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

Degree
------

Comparatives and superlatives are represented in DeAMR almost the same way as in AMR. You use the same frame `have-degree-91` but match the German attributes and the degree itself.

```lisp
Have-degree-91
Arg1: domain, entity characterized by attribute (e.g. Hund)
Arg2: attribute (e.g. klein)
Arg3: degree itself (e.g. mehr, weniger, gleich, am-meisten, am-wenigsten, genug, mal)
Arg4: compared-to (e.g. (wie die) Katze)
Arg5: superlative: reference to superset
Arg6: reference, threshold of sufficiency (e.g. (klein genug) um im Auto zu sitzen)
```

Compounds
---------

In German, there are many different ways of combining different word classes into new words. If you want to annotate a compound, try and follow this approach:

1. Look up German PropBank and see if the compound word exists (e.g. “nachgesehen-01”)
```

If exists: use this as your annotation
Else: Continue below

```

2. Evaluate if the respective compound is lexicalized or not. It might be better to use intuition rather than fixed rules when a word combination is either productive or nonproductive.

```

If word combination nonproductive: use the whole word as it is in your annotation (without numbers).

Elif word combination productive: Lift the semantic head up to the top node of the compound subgraph and try to find a fitting semantic role for the modifier component; if there is no adequate semantic role, use :mod.

```

Noun+noun

```lisp
(c3 / ähneln-01
   :ARG1 (c0 / ausbrechen-02
             :ARG1 (c1 / Vulkan))
   :ARG2 (c2 / Feuer
             :location (c4 / Kamin)))
```
> Vulkanische Ausbrüche sind wie Kaminfeuer

```lisp
(d / zeichnen-01
      :ARG0 (i / ich)
      :ARG1 (m / Maulkorb
            :poss (s / Schaf
                        :poss y))
      :ARG2 (y / du))
```
> Ich zeichne dir einen Maulkorb für dein Schaf.



Adjectives evoking verb frame
-----------------------------
One of AMRs slogans is to prefer a verb frame when it is possible.

