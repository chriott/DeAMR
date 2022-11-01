# DeAMR 1.0 (WIP 🚧)
These are the annotation guidelines for DeAMR (German AMR), which are build in alignment with the official [AMR guidelines](https://github.com/amrisi/amr-guidelines/blob/master/amr.md). The aim is to extend AMR's functionality so that German linguistic phenomena can be sufficiently mapped into AMR form.


**Table of Contents**
- [Introduction](#introduction-🌱)
    - [Abstract Meaning Representation](#abstract-meaning-representation-amr-💭)
    - [Verb Senses](#verb-senses-🔡)
- [Annotation Guidelines](#annotation-guidelines-🗂️)
    - [Compounds](#compounds-🧩)   
    - [Coordination and Clausal connectives](#coordination-and-clausal-connectives-🧩)
    - [Degree](#degree-🧩)
    - [Special dashed entities / relations](#special-dashed-entities-and-relations-🧩)
    - [Modality](#modality-🧩)
    - [Modal particles](#modal-particles-🧩) 
- [References](#references-📚)


Introduction 🌱
====================
Abstract Meaning Representation (AMR) 💭
----

AMR is a semantic formalism that captures information on who is doing what to whom in a sentence. DeAMR is its German adaptation. AMR can be visualized as a rooted, directed, acyclic graph. The edges are relations. Each node in the graph has a variable and they are labeled with concepts:

<p align="center">
<img src="https://i.ibb.co/djVMtR8/Bildschirmfoto-2022-10-20-um-21-30-43.png" width=60% height=100%>
</p>

AMR and DeAMR lean heavily on PENMAN-notation, which is a way of representing a graph in a simple, tree-like form:

```lisp
(c1 / zeichnen-01
    :mode imperative
    :ARG0 (c0 / du)
    :ARG1 (c3 / Schaf)
              :mod (c4 / weiß)
    :ARG2 (c2 / ich))
```
Both representations could be rendered into the German sentence: 

> Zeichne mir ein weißes Schaf!

Verb Senses 🔡
-----------
DeAMR is using the German frame set from the [Universal PropBank](https://universalpropositions.github.io) project and their searchable [German PropBank catalogue](http://alanakbik.github.io/UniversalPropositions_German/index.html).

The Universal German PropBank is still in development and incomplete. Meanwhile, if a German frame for a concept should be missing, e.g. *zufrieden*, one workaround could be to use the German concept without numbers and find a fitting original PropBank frame to align the argument structure, e.g. for *zufrieden* use [content-01](https://verbs.colorado.edu/propbank/framesets-english-aliases/content.html). A list of the original PropBank frames can be found [here](https://verbs.colorado.edu/propbank/framesets-english-aliases) and [here](https://propbank.github.io/v3.4.0/frames/). For functional roles see [here](https://www.isi.edu/~ulf/amr/lib/roles.html).

Sometimes, a missing German frame can be replaced with a similar existing German frame. If so, this variant should always be preferred to creating a new frame. For example: 

```lisp
(c2 / stammen-01
    :ARG1 (c3 / du
              :mod (c5 / Mann
                       :degree (c6 / klein)))
    :ARG2 (c4 / amr-unknown))
```
> Wo kommst du her, kleiner Mann?

"Herkommen" does not exist in the German PropBank. `stammen-01` holds a compatible semantic meaning and argument structure so it represents a good alterantive.

Annotation Guidelines 🗂️ 
===

This version of the DeAMR guidelines tries to provide the first pieces of a puzzle to cover the full range of linguistic phenomena of German.

Adjectives/Adverbs evoking a verb frame 🧩
---

One of AMRs slogans is to prefer a verb frame whenever it is possible.

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

Here, the adverb *verwirrt* evokes the verb frame `verwirren-01` and thus should be used in the annotation.

Degree 🧩
---

Comparatives and superlatives are represented in DeAMR almost the same way as in AMR. You use the same frame `have-degree-91` but match the German attributes and the degree itself.

```
Have-degree-91
Arg1: domain, entity characterized by attribute (e.g. Hund)
Arg2: attribute (e.g. klein)
Arg3: degree itself (e.g. mehr, weniger, gleich, am-meisten, am-wenigsten, genug, mal)
Arg4: compared-to (e.g. (wie die) Katze)
Arg5: superlative: reference to superset
Arg6: reference, threshold of sufficiency (e.g. (klein genug) um im Auto zu sitzen)
```

Compounds 🧩
---

In German, there are multiple ways of combining different word classes into new words. If you want to annotate such a compound, follow this approach:

I. Look up German PropBank and see if the compound word exists (e.g. "nachgesehen-01")
```

if exists: use this as your annotation
else: continue below

```

II. Evaluate if the respective compound is lexicalized or not. It might be better to use intuition rather than fixed rules to determine wheter a word combination is productive or nonproductive.

```

if word combination nonproductive: use the whole word as it is in your annotation (without numbers).

elif word combination productive: lift the semantic head up to the top node of the compound subgraph and try to find a fitting semantic role for the modifier component; if there is no adequate semantic role, use :mod.

```

The following examples should provide an intuition:

```lisp
(c3 / ähneln-01
   :ARG1 (c0 / ausbrechen-02
             :ARG1 (c1 / Vulkan))
   :ARG2 (c2 / Feuer
             :location (c4 / Kamin)))
```
> Vulkanische Ausbrüche sind wie Kaminfeuer

`Feuer` occurs in a set of different compounds (Artilleriefeuer, Lagerfeuer, Martinsfeuer, Fegefeuer, Grillfeuer, etc.) and therefore seems to be productive. According to the guidelines, we lift the semantic head `Feuer` up and match it with a fitting semantic role `:location` for the modifier `Kamin`.

```lisp
(d / zeichnen-01
      :ARG0 (i / ich)
      :ARG1 (m / Maulkorb
            :poss (s / Schaf
                        :poss y))
      :ARG2 (y / du))
```
> Ich zeichne dir einen Maulkorb für dein Schaf.

"Maulkorb" could be declared as a more lexicalized word and thus not split up by semantic roles.

Coordination and Clausal connectives 🧩
---

Example ENG/DE | AMR | DeAMR
--------|-----|------
and/und     | and | und
or/oder      | or  | oder
but/aber     | `:contrast-01` | `:contrast-01`
because/weil; due to/wegen; on account of/aufgrund von | `:cause-01` | `:cause-01`
(in order) to/damit; so (that)/sodass | `:purpose` | `:purpose`
if/wenn | `:condition` | `:condition`
unless/außer | `:condition`, `:polarity` | `:condition`, `:polarity`
altough/obwohl; despite/trotz | `:concession`| `:concession`


Special dashed entities and relations 🧩
---

For special and funtional roles (such as `have-degree-91`) DeAMR uses the English terms.

Modality 🧩
---

AMR represents syntactic modals with concepts like `possible-01`, `likely-01`, `obligate-01`, `permit-01`, `recommend-01`, `prefer-01`, etc. DeAMR tries to capture Modality in the same way using equivalent German frames:

English modal verb     |     PropBank       | German modal verb   | German PropBank   | Example
-----------------------|--------------------|---------------------|-------------------|---------
`may`                  | `permit-01`, `possible-01` | `dürfen`| `erlauben-01`, `befähigen-01`     | “They may go” / "Sie dürfen gehen"
`can`                  | `possible-01`      | `können`            | `ermöglichen-01`, `können-01`  | “He can swim” / "Er kann schwimmen"
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
> Er will essen.
> 
> Er möchte essen.

Modal particles 🧩
---

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

# References 📚

#Bin Li, YuanWen, Lijun Bu, Weiguang Qu and Nianwen Xue. Annotating the Little Prince with Chinese AMRs. LAW-2016, Aug 11, 2016, Berlin, Germany.

#Nathan Schneider, Tim O'Gorman and Jeffrey Flanigang. [AMR Tutorial](https://github.com/nschneid/amr-tutorial/tree/master/slides) presented at NAACL 2015.

#Nianwen Xue, Chuan Wang, Yuchen Zhang, Bin Li, Lijun Bu, Yuan Wen, Li Song, Rubing Dai, Junsheng Zhou and Weiguang Qu. CAMR Guidelines [v1.2](https://www.cs.brandeis.edu/~clp/camr/res/CAMR_GL_v1.2.pdf).

