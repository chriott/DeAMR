# DeAMR (WIP)
These are the annotation guidelines for DeAMR (German AMR), which aim to work in alignment to the official [AMR guidelines](https://github.com/amrisi/amr-guidelines/blob/master/amr.md).

(WIP in vsc)

**Table of Contents**
- [Introduction](#introduction)
   - [Abstraction away from English](#Abstraction-away-from-English)
   - [Verb Senses](#verb-senses)
- [Annotation Guidelines](#annotation-guidelines)
   - [Compounds](#compounds)
   - [Degree](#degree)
   - [Modality](#modality)
   - [Modal particles](#modal-particles) 
- [References](#references)


Introduction
====================

```lisp
(c1 / zeichnen-01
    :mode expressive
    :ARG0 (c0 / du)
    :ARG1 (c3 / Schaf)
    :ARG2 (c2 / ich)
    :location (c4 / Papier))
```
> Zeichne mir ein Schaf auf Papier!

Verb Senses
-----------
I am using the German frame set from the [Universal PropBank](https://universalpropositions.github.io) project and their searchable [German PropBank catalogue](http://alanakbik.github.io/UniversalPropositions_German/index.html).

The Universal German PropBank is still in development and incomplete. Therefore, if a German frame for a concept should be missing, e.g. "zufrieden", one workaround could be to use the German concept without numbers and find a fitting original PropBank frame to align the argument structure, e.g. for "zufrieden" use "[content-01](https://verbs.colorado.edu/propbank/framesets-english-aliases/content.html)". A list of the original PropBank frames can be found [here](https://verbs.colorado.edu/propbank/framesets-english-aliases) and [here](https://propbank.github.io/v3.4.0/frames/).

Sometimes, a missing German frame can be replaced with a similar existing German frame. If so, this variant should always be preferred to creating a new frame leaning on the original PropBank and without numbers. For example: 

```lisp
(c2 / stammen-01
    :ARG1 (c3 / du
              :mod (c5 / Mann
                       :degree (c6 / klein)))
    :ARG2 (c4 / amr-unknown))
```
> Wo kommst du her, kleiner Mann?

"Herkommen" does not exist in the German PropBank. "Stammen-01" holds a compatible semantic meaning and argument structure so it represents a good alterantive.

Annotation Guidelines
====================

Adjectives evoking verb frame
-----------------------------
One of AMRs slogans is to prefer a verb frame when it is possible.

```lisp
(c0 / entgegnen-01
    :ARG0 (c1 / Prinz
              :mod (c2 / klein))
    :ARG1 (c4 / ermöglichen-01
              :polarity (a5 / -)
              :manner (c6 / anders)
              :ARG0 c1)
    :manner (c7 / verwirren-01
                :mod (c8 / ganz)))
```
> "Ich kann aber nicht anders", entgegnete der kleine Prinz ganz verwirrt.


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

Modality 
--------

AMR represents syntactic modals with concepts like possible-01, likely-01, obligate-01, permit-01, recommend-01, prefer-01, etc. DeAMR tries to capture Modality in the same way using equivalent German frames:

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

German has a large set of different particles. The subset of modal particles are annotated in a way that captures the semantics of the - sometimes convoluted - interaction between the particle itself and the grammatical mood. Here is a table that presents a range of different possible contexts and the corresponding annotation:


Modal particle          | Context                                          | Annotation   
------------------------|--------------------------------------------------|----------------------------------
`doch`, `halt`, `ja`, `eben`, `wohl` | "Man weiß halt nie.", "Das ist doch falsch."                             | `:mode conclusive`
`doch`, `aber`, `wohl`, `eben`, `halt`, `ja` | "Ich habe schon nachgesehen.", "Das ist schon richtig."  | `:mode confirming`
`aber`, `doch`, `schon`, `ja`, `auch` | "Du bist aber schnell!"                          | `:mode surprised`
`aber`, `doch`, `schon`, `ja`, `auch` | "Das machst du aber gut!"                        | `:mode sarcasm`, `:mode irony`
`eigentlich`, `doch`, `denn`, `bloß`, `auch`, `etwa` | "Kannst du eigentlich Gitarre spielen?"                        | `:mode confirmation-seeking`
`auch`, `bloß`, `doch`, `eben`, `einfach`, `halt`, `mal`, `nur`, `schon`, `ruhig`| "Mach bloß das Fenster zu!"   | `:mode imperative`


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

```lisp
(c1 / zumachen-01
    :mode imperative
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

References WIP
==============

Bin Li, YuanWen, Lijun Bu,Weiguang Qu, Nianwen Xue. Annotating the Little Prince with Chinese AMRs. LAW-2016, Aug 11, 2016, Berlin, Germany.

CAMR Guidelines [v1.2](https://www.cs.brandeis.edu/~clp/camr/res/CAMR_GL_v1.2.pdf)

