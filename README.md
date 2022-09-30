# DeAMR
Annotation guidelines for German AMR.

**September 1, 2022**

(WIP in vsc)

Reference CAMR+ Spanish AMR

**Table of Contents**
- [Introduction](#introduction)
   - [Abstraction away from English](#Abstraction-away-from-English)
   - [Verb Senses](#verb-senses)
- [Annotation Guidelines](#annotation-guidelines)

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

Modality 
--------

Reference: Note the presence of deber and poder in the list of words which appear in AnCora with other senses. Though meanings of deber and poder do appear in AnCora, we establish additional senses to mark modality. These modals take the same :ARG1 structure as do their English modal equivalents– recommend-01 and possible-01, respectively. These modals take the verb senses deber-03 and poder-04.

