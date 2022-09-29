# DeAMR
Annotation guidlines for German AMR


Concepts & Relations
German concept annotation:
Look up German PropBank to see if a sense for the concept in questions exists
Yes: use this as your annotation z.B (e.g. “altern-01”)
No: continue below
See if there exists something similar 
Yes: use this as your annotation
No: continue below
Use the stem of the concept you could not find and use it without sense numbering following the OntoNotes system (Weischedel et al. 2012)
Arg0 = external argument (Proto-Agent)
Arg1 = internal argument (Proto-Patient)
Arg2 = indirect object / beneficiary / instrument / attribute / end state
Arg3 = start point / beneficiary / instrument / attribute
Arg4 = end point
Conjunctions

To represent conjunctions, GAMR, just like AMR, uses concepts “and”, “or”, “contrast-01”, “either”, and “neither”, along with :opx relations.
Compound words

Lexicalized words (Affenbrotbaum) shouldn’t be additionally annotated as opposed to ‘less’ lexicalized words (Glasglocke)
Compound words that have a Propbank entry e.g. “nachgesehen-01” are used and preferred that way 
Translation pairs that have no corresponding German verb frame, such as “quiet sweetness”/“Sanftmut” are not annotated separately
AMR guidelines 1.2.6: “when we build AMR from text, we generally do not introduce implicit concepts”
LLP consists mostly of lexicalized compounds - it is “easier” or more intuitive to decide if something is lexical so I assume that this will be mostly the case while annotating. 

Compound annotation:
Look up German PropBank and see if the compound word exists (e.g. “nachgesehen-01”)
Yes: use this as your annotation
No: continue below
Evaluate whether the compound is lexicalized or not (productive vs. nonproductive; intuition rather than rigid rules to account for changes in language)
Nonproductive intuition: use the lexicalized word as your annotation (without numbers)
Productive intuition: Lift semantic head up to the top node of the compound subgraph and try to find a fitting non-core role for the modifier component;
no fitting non-core role: use :mod

The fourth planet belonged to a businessman
Der vierte Planet gehörte einem Geschäftsmann

(c0 / gehören-01
   :ARG0 (c2 / Planet
             :mod (c3 / vierte))
   :ARG1 (c1 / Geschäftsmann))

(c0 / have-quant-91
   :ARG1 (c1 / Platz)
   :ARG3 (c2 / genug
             :mod (c3 / gerade))
   :ARG6 (c4 / unterbringen-01
             :ARG0 c1
             :ARG1 (c5 / und
                       :op1 (c6 / Straßenlaterne)
                       :op2 (c7 / Laternenanzünder))))


Vulkanische Ausbrüche sind wie Kaminfeuer
Volcanic eruptions are like fires in a chimney

(c3 / ähneln-01
   :ARG1 (c0 / ausbrechen-02
             :ARG1 (c1 / Vulkan))
   :ARG2 (c2 / Feuer
             :location (c4 / Kamin)))

Term itself is more or less lexicalized but the head component is productive


Noun + noun
1.) 

German AMR

Für mich war es eine Frage von Leben oder Tod: Ich hatte kaum genug Trinkwasser für eine Woche

(q / fragen-01
      :ARG0 (i /ich)
      :ARG1 (o / oder
            :op1 (l / leben-01)
            :op2 (d / sterben-01))
      :ARG1-of (c / verursachen-01
            :ARG0 (h / haben-01
                  :ARG0 (i / ich)
                  :ARG1 (w / Trinkwasser
                        :ARG1-of (h2 / have-quant-91
                              :ARG3 (g / genug
                                    :mod (s / kaum))
                              :ARG6 (l2 / andauern-01
                                    :ARG2 (t / temporal-quantity :quant 1
                                          :unit (w2 / woche))
                                    :ARG3 i))))))

AMR

# ::id lpp_1943.40 ::date 2012-07-24T10:37:48 ::annotator ISI-AMR-05 ::preferred
# ::snt It was a question of life or death for me : I had scarcely enough drinking water to last a week .
# ::save-date Fri Sep 22, 2017 ::file lpp_1943_40.txt
(q / question-01
      :ARG0 i
      :ARG1 (o / or
            :op1 (l / live-01)
            :op2 (d / die-01))
      :ARG1-of (c / cause-01
            :ARG0 (h / have-03
                  :ARG0 (i / i)
                  :ARG1 (w / water
                        :purpose (d2 / drink-01)
                        :ARG1-of (h2 / have-quant-91
                              :ARG3 (e / enough
                                    :mod (s / scarce))
                              :ARG6 (l2 / last-03
                                    :ARG2 (t / temporal-quantity :quant 1
                                          :unit (w2 / week))
                                    :ARG3 i))))))

2.) 

German AMR

Ich zeichne dir einen Maulkorb für dein Schaf

(d / zeichnen-01
      :ARG0 (i / ich)
      :ARG1 (m / Maulkorb
            :poss (s / Schaf
                        :poss y))
      :ARG2 (y / du))

AMR

# ::id lpp_1943.360 ::date 2012-11-24T08:16:28 ::annotator ISI-AMR-05 ::preferred
# ::snt I will draw you a muzzle for your sheep .
# ::save-date Tue Apr 23, 2013 ::file lpp_1943_360.txt
(d / draw-01
      :ARG0 (i / i)
      :ARG1 (t / thing
            :ARG2-of (m / muzzle-01
                  :ARG1 (s / sheep
                        :poss y)))
      :ARG2 (y / you))

always search for predicates even if the noun is more frequent (thing that muzzles vs. muzzle)

3.) 


German AMR

Ganz verwirrt stand er mit der Glasglocke da (Kirchenglocke, Schulglocke, Rauchglocke, Glasglocke, Schutzglocke,...)


(s / stehen-01
      :ARG1 (e / er
            :ARG1-of (v / verwirren-01
                  :degree (g / ganz))
            :ARG0-of (h / halten-01
                  :ARG1 (g2 / Glocke
                        :consist-of (g3 / Glas)
      :ARG2 (d / da))

:consist-of vs. :mod 

AMR

# ::id lpp_1943.457 ::date 2012-11-11T15:05:03 ::annotator ISI-AMR-05 ::preferred
# ::snt He stood there all bewildered , the glass globe held arrested in mid - air .
# ::save-date Mon Feb 22, 2016 ::file lpp_1943_457.txt
(s / stand-01
      :ARG1 (h / he
            :ARG1-of (b / bewilder-01
                  :degree (a / all))
            :ARG0-of (h2 / hold-01
                  :ARG1 (g / globe
                        :consist-of (g2 / glass)
                        :ARG1-of (a2 / arrest-02
                              :location (m / midair)))))
      :ARG2 (t / there))


Adjective + noun

1.)

German AMR

Doch diesen Sanftmut verstand er nicht

(u / verstehen-01 :polarity -
      :ARG0 (e / er)
      :ARG1 (s / Sanftmut
            :mod (d / diesen)

Difficult to align propbank frames with translation
Look up stem form

AMR

# ::id lpp_1943.458 ::date 2012-11-11T15:21:43 ::annotator ISI-AMR-05 ::preferred
# ::snt He did not understand this quiet sweetness .
# ::save-date Tue Dec 1, 2015 ::file lpp_1943_458.txt
(u / understand-01 :polarity -
      :ARG0 (h / he)
      :ARG1 (s / sweet-05
            :ARG1 (t / this)
            :ARG1-of (q / quiet-04)))

Noun + adjective

1.)

German AMR

Es ist so geheimnisvoll, das Land der Tränen

(g / geheimnisvoll)

(l / land
      :mod (s / geheimnisvoll
            :degree (s / so))
      :location-of (T / Tränen)

AMR

# ::id lpp_1943.366 ::date 2012-11-24T08:33:09 ::annotator ISI-AMR-05 ::preferred
# ::snt It is such a secret place , the land of tears .
# ::save-date Tue Apr 23, 2013 ::file lpp_1943_366.txt
(p / place
      :mod (s / secret
            :degree (s2 / such))
      :domain (l / land
            :location-of (t / tear)))




Adjective + adjective

1.)

German AMR

Wenn sie gut gefegt werden, brennen die Vulkane sanft und gleichmäßig, ohne jemals auszubrechen.

(b / brennen-01
      :ARG1 (v / Vulkan
            :ARG1-of (a / ausbrechen-02 :polarity -))
      :condition (c / fegen-01
            :ARG1 v
            :degree (w / gut))
      :manner (g / gleichmäßig)
      :manner (l / langsam)




AMR

# ::id lpp_1943.440 ::date 2012-11-11T13:11:43 ::annotator ISI-AMR-05 ::preferred
# ::snt If they are well cleaned out , volcanoes burn slowly and steadily , without any eruptions .
# ::save-date Fri Nov 13, 2015 ::file lpp_1943_440.txt
(b / burn-01
      :ARG1 (v / volcano
            :ARG1-of (e2 / erupt-01 :polarity -))
      :condition (c / clean-out-03
            :ARG1 v
            :degree (w / well))
      :manner (s3 / steady)
      :ARG1-of (s / slow-05))

2.)

German AMR

Mein Hammer, mein Bolzen, Durst und Tod waren mir gleichgültig.

(o / or
      :op1 (h / Hammer
            :poss (i / ich))
      :op2 (b / Bolzen
            :poss i)
      :op3 (t / durst-01
            :ARG0 i)
      :op4 (d / sterben-01
            :ARG1 i)
      :manner (g / gleichgültig)

AMR

# ::id lpp_1943.356 ::date 2012-11-24T07:59:49 ::annotator ISI-AMR-05 ::preferred
# ::snt Of what moment now was my hammer , my bolt , or thirst , or death ?
# ::save-date Tue Apr 23, 2013 ::file lpp_1943_356.txt
(o / or
      :op1 (h / hammer
            :poss (i / i))
      :op2 (b / bolt
            :poss i)
      :op3 (t / thirst-01
            :ARG0 i)
      :op4 (d / die-01
            :ARG1 i)
      :time (m / moment
            :mod (a / amr-unknown)))

Verb stem + noun

Preposition + verb

1.)

German AMR

»Oh! Aber ich habe schon nachgesehen«, sagte der kleine Prinz und drehte sich um, um noch einen weiteren Blick auf die andere Seite des Planeten zu werfen.

(s / sagen-01
      :ARG0 (p / Prinz
            :mod (l / klein)
            :ARG0-of (t / drehen-01
                  :ARG1 p
                  :purpose (g / blicken-01 :quant 1
                        :ARG0 p
                        :ARG1 (s2 / Seite
                              :mod (o / andere)
                              :part-of (p2 / Planet))
                        :mod (m / more))))
      :ARG1 (c / contrast-01
	:mode expressive
            :ARG2 (l2 / nachgesehen-01
                  :ARG0 p

AMR 

# ::id lpp_1943.574 ::date 2012-11-25T18:40:18 ::annotator ISI-AMR-05 ::preferred
# ::snt " Oh , but I have looked already ! " said the little prince , turning around to give one more glance to the other side of the planet .
# ::save-date Tue May 20, 2014 ::file lpp_1943_574.txt
(s / say-01
      :ARG0 (p / prince
            :mod (l / little)
            :ARG0-of (t / turn-01
                  :ARG1 p
                  :purpose (g / glance-01 :quant 1
                        :ARG0 p
                        :ARG1 (s2 / side
                              :mod (o / other)
                              :part-of (p2 / planet))
                        :mod (m / more))))
      :ARG1 (c / contrast-01
            :ARG2 (l2 / look-01
                  :ARG0 p


Preposition + noun

1.)

German AMR

Von ernsten Dingen hatte der kleine Prinz eine ganz andere Vorstellung als die Erwachsenen.

(h / haben-03
      :ARG0 (p / Prinz
            :mod (l / klein))
      :ARG1 (i / Vorstellung
            :topic (f / Ding
                  :ARG1-of (e / ernst))
            :ARG1-of (d / unterscheiden-01
                  :ARG1 (e / Erwachsene))
                  :degree (g / ganz))))

AMR

# ::id lpp_1943.746 ::date 2012-11-13T12:26:16 ::annotator ISI-AMR-05 ::preferred
# ::snt On matters of consequence , the little prince had ideas which were very different from those of the grown - ups .
# ::save-date Thu Nov 2, 2017 ::file lpp_1943_746.txt
(h / have-03
      :ARG0 (p / prince
            :mod (l / little))
      :ARG1 (i / idea
            :topic (m / matter
                  :ARG1-of (c / consequential-01))
            :ARG1-of (d / differ-02
                  :ARG2 (i2 / idea
                        :poss (g / grown-up))
                  :degree (v / very))))


Modal particles

express the speaker’s attitude towards a proposition
refer to the addressee’s knowledge or common knowledge
place the content of a sentence on a continuum from possible to impossible
indicate the speaker’s judgment of the discourse situations
e.g. ja, denn, doch, aber, nur, halt, eben, mal, schon, auch, bloß, eigentlich, etwa, nicht, vielleicht, ruhig
Modal particles can be combined: doch mal, ja nun, ja doch
Depending on the context, modal particles can be rendered into different types of “meaning”
aber: speaker indicates that what she is saying is unexpected for her
auch: establishes a relationship between old and new information
nur: in optatives used to indicate there were contrary expectations or hopes; in imperative the speaker wants so signal that there are no obstacles to act in a certain way. In interrogative constructions
bloß: see nur

v.0.1

Modal particle
Context
Annotation
doch, halt, schon
Komm doch her!
(“Come here!”)
Man weiß ja nie.
(“You never know.”)
:mode emphasis
eh, schon, ja, halt, eben, doch, aber, wohl
Das ist eh gelogen.
(“That’s a lie anyway.”)
Ich habe schon nachgesehen.
(“I have looked already.”)

Er sagt das ja selbst. (“He says so himself.”)
Es ist halt so.
(“It’s just the way it is.”)
Es wird wohl regnen.
(“It’s going to rain.”)
:mode assertive



aber, doch
Du bist aber schnell!
(“You’re quick!'')
Du bist doch gekommen (“You came after all.”)
:mode surprised
aber
Das machst du aber gut!
(“You’re doing a good job!)
:mode sarcasm/irony
Einmal / Mal
Hör mal zu!
(“Listen to me!”)
:mode imperative








v.1.0:




“pragmatic function” → MPs evoke a speech situation in which a speaker can justify an intended speech act 
Mood: form of a verb → wish, command, desire, factual expression
:mode indicative (declarative+(polar+w-)interrogative), :mode subjunctive (optative, modality?), :mode expressive (exclamatory, w-exclamatory)
further branch out vs. 3 grammatical moods

German AMR 

Oh, aber ich habe schon nachgesehen!

»Oh, aber ich habe schon nachgesehen!«, sagte der kleine Prinz und drehte sich um, um noch einen weiteren Blick auf die andere Seite des Planeten zu werfen.

(s / sagen-01
      :ARG0 (p / Prinz
            :mod (l / klein)
            :ARG0-of (t / drehen-01
                  :ARG1 p
                  :purpose (g / blicken-01 :quant 1
                        :ARG0 p
                        :ARG1 (s2 / Seite
                              :mod (o / andere)
                              :part-of (p2 / Planet))
                        :mod (m / more))))
      :ARG1 (c / contrast-01
	 :mode declarative
            :ARG2 (l2 / nachgesehen-01
                  :ARG0 p)))

AMR

# ::id lpp_1943.574 ::date 2012-11-25T18:40:18 ::annotator ISI-AMR-05 ::preferred
# ::snt " Oh , but I have looked already ! " said the little prince , turning around to give one more glance to the other side of the planet .
# ::save-date Tue May 20, 2014 ::file lpp_1943_574.txt
(s / say-01
      :ARG0 (p / prince
            :mod (l / little)
            :ARG0-of (t / turn-01
                  :ARG1 p
                  :purpose (g / glance-01 :quant 1
                        :ARG0 p
                        :ARG1 (s2 / side
                              :mod (o / other)
                              :part-of (p2 / planet))
                        :mod (m / more))))
      :ARG1 (c / contrast-01
            :ARG2 (l2 / look-01
                  :ARG0 p


# ::id 50-selection-parallel.50 ::annotator 1
# ::snt Aber er sagte sich : Man weiß ja nie !
(c0 / contrast-01
    :ARG2 (c2 / sagen-01
              :ARG0 (c1 / er)
              :ARG1 (c3 / wissen-01
		 :mode emphasis
                        :polarity -
		 :mod (j / ja)
                        :ARG0 (c6 / man))
              :ARG2 c1))

# ::id lpp_1943.1001 ::date 2012-11-18T20:01:24 ::annotator ISI-AMR-05 ::preferred
# ::snt But one never knows where to find them .
# ::save-date Tue Jan 23, 2018 ::file lpp_1943_1001.txt
(c / contrast-01
      :ARG2 (k / know-01 :polarity -
            :ARG0 (o / one)
            :ARG1 (l / location
                  :location-of (f / find-01
                        :ARG0 o
                        :ARG1 (t / they)))
            :time (e / ever)))

# ::id 50-selection-parallel.50 ::annotator 1
# ::snt Man weiß ja nie, wo man sie findet .
(c / wissen-01 :polarity -
      :mode assertive
      :ARG0 (o / Man)
      :location (f / finden-01
                   :ARG0 o
                   :ARG1 (t / sie)))))

# ::id lpp_1943.735 ::date 2012-11-12T16:36:13 ::annotator ISI-AMR-05 ::preferred
# ::snt But you can not pluck the stars from heaven ... "
# ::save-date Thu May 9, 2013 ::file lpp_1943_735.txt
(c / contrast-01
  :ARG2 (p2 / possible-01
          :polarity -
          :ARG1 (p / pluck-01
                    :ARG0 (y / you)
                    :ARG1 (s / star)
                    :ARG2 (h / heaven))))

# ::id 50-selection-parallel.50 ::annotator 1
# ::snt Aber du kannst die Sterne ja nicht pflücken !
(c / contrast-01
  :ARG2 (p2 / possible-01
          :mode emphasis
          :polarity -
          :ARG1 (p / pflücken-01
                    :ARG0 (y / du)
                    :ARG1 (s / Sterne))))






Negation

1.)

German uses “nicht” to negate verbs, adjectives, adverbs, pronouns, prepositions, proper names, nouns with definite articles/possessive determiners and complete sentences.
German uses “kein” to negate nouns with indefinite/without articles
“nein” is only used as an negative answer to a question

nicht

German AMR

Ich bin nicht gezähmt.

( z / zähmen-01
  :ARG1 (i / ich)
  :polarity -)

AMR

# ::id lpp_1943.1052 ::date 2012-11-11T14:47:18 ::annotator ISI-AMR-05 ::preferred
# ::snt " I am not tamed . "
# ::save-date Sun Nov 11, 2012 ::file lpp_1943_1052.txt
(t / tame-01
  :ARG1 (i / i)
  :polarity -)

2.)

kein

German AMR
»Sieh doch … das ist kein Schaf, es ist ein Widder. Er hat Hörner …«
(s / sagen-01
  :ARG0 (e / er)
  :ARG1 (s2 / sehen-01
          :ARG0 (d / du)
          :ARG1 (s4 / Schaf
                  :domain (t2 / das)
                  :polarity -)))


AMR

# ::id lpp_1943.81 ::date 2012-07-24T13:31:13 ::annotator ISI-AMR-05 ::preferred
# ::snt " You see yourself , " he said , " that this is not a sheep .
# ::save-date Mon Apr 15, 2013 ::file lpp_1943_81.txt
(s / say-01
  :ARG0 (h / he)
  :ARG1 (s2 / see-01
          :ARG0 (y / you)
          :ARG1 (s4 / sheep
                  :domain (t2 / this)
                  :polarity -)))

3.)

German AMR
»Nein! Aber nein! Ich glaube nichts. Ich habe das nur so gesagt. Ich habe nur gerade wichtigere Dinge zu tun!«
(n / nein
      :mod (o / oh :mode expressive))

AMR 

# ::id lpp_1943.317 ::date 2012-11-19T12:07:38 ::annotator ISI-AMR-05 ::preferred
# ::snt " Oh , no ! "
# ::save-date Wed May 15, 2013 ::file lpp_1943_317.txt
(n / no
      :mod (o / oh :mode expressive))


Modality

GAMR, just like AMR, represents syntactic modals with concepts like possible-01, likely-01, obligate-01, permit-01, recommend-01, prefer-01, etc.

In German there are 6 modal verbs: “dürfen” - “erlauben-01”, “sollen”, “müssen”, “wollen” and “mögen”.

In agreement to the practice of CAMR and Spanish AMR I want to use modal verb frames as follows


Appendix 50 AMR selection:

# ::id 50-selection-test.1 ::annotator 1
# ::snt Er antwortete mir : " Das macht nichts . "
(c0 / antworten-01
    :ARG0 (c4 / er)
    :ARG1 (c6 / ich)
    :ARG2 (c12 / machen-01
               :ARG1 (c14 / das)
               :polarity -))

# ::id 50-selection-test.3 ::annotator 1
# ::snt Zeichne mir ein Schaf .
(c0 / zeichnen-01
    :mode  imperative 
    :ARG1 (c2 / Schaf)
    :ARG2 (c1 / ich))

# ::id 50-selection-test.5 ::annotator 1
# ::snt Da , wo ich lebe , ist alles sehr klein .
(c0 / klein
    :degree (s / sehr)
    :domain (a / alles)
    :location (c4 / leben-01
                  :ARG0 (c6 / ich)))

# ::id 50-selection-test.7 ::annotator 1
# ::snt Das ist lustig !
(c0 / lustig
    :domain (c1 / das))


# ::id 50-selection-test.9 ::annotator 1
# ::snt Von welchem Planeten bist du denn her ?
(c0 / kommen-01
    :ARG1 (c1 / du)
    :ARG3 (c2 / Planet)
    :domain (c3 / amr-unknown))

# ::id 50-selection-test.9 ::annotator 1
# ::snt Von welchem Planeten bist du denn her ?
(c4 / Planet
    :poss (d / du)
    :domain (c6 / amr-unknown))

Comment: Translation → “kommen-01” equal to “bist du her”; fully utilize UP or try to find an “intermediate” layer ; Do I even include modal particle here ?


# ::id 50-selection-parallel.23 ::annotator 1
# ::snt Wenn ein Astronom einen von ihnen entdeckt , so gibt er ihm eine Nummer anstelle eines Namens .
(c0 / geben-01
    :ARG0 (c3 / Astronom)
    :ARG1 (c1 / Nummer)
    :ARG1-of (c4 / instead-of-91
                 :ARG2 (c5 / benennen-01
                           :ARG0 c3))
    :time (c10 / entdecken-01
               :ARG1 c11
               :ARG0 c3)
    :ARG2 (c11 / thing
               :quant 1
               :ARG1-of (c13 / include-91
                             :ARG2 (c14 / this))))

# ::id 50-selection-parallel.26 ::annotator 1
# ::snt Kinder müssen mit Erwachsenen nachsichtig sein .
(c0 / empfehlen-01
    :ARG0 (c3 / Kind)
    :ARG1 (c5 / nachsichtig)
    :ARG2 (c4 / Erwachse))

# ::id 50-selection-parallel.29 ::annotator 1
# ::snt Aber wir , die das Leben verstehen , lachen natürlich über Zahlen !
(c0 / contrast-01
    :ARG1 (c6 / wir
              :ARG0-of (c2 / verstehen-01
                           :ARG1 (c4 / Leben))
              :ARG0-of (c5 / lachen-01
                           :ARG2 (c7 / Zahl)
                           :mod (c8 / natürlich))))

Comment: adjectives as :mod?

# ::id 50-selection-parallel.32 ::annotator 1
# ::snt Es ist traurig einen Freund zu vergessen .
(c4 / traurig
    :ARG0 (c0 / vergessen-01
              :ARG1 (c1 / Freund
                        :quant 1)))

# ::id 50-selection-parallel.35 ::annotator 1
# ::snt Nicht jeder hatte schon mal einen Freund .
(c4 / haben-01
    :polarity -
    :ARG0 (c1 / jeder)
    :ARG1 (c0 / Freund
              :quant 1))

# ::id 50-selection-parallel.38 ::annotator 1
# ::snt Aber ich bin nicht sicher , ob ich es schaffe .
(c0 / contrast-01
    :ARG2 (c3 / sicherstellen-01
              :polarity -
              :ARG0 (c1 / ich)
              :ARG1-of (c7 / schaffen-02
                           :ARG0 c1)))

Comment: “contrast-01” or “aber”
Comment: sicher vs. sicherstellen-01

# ::id 50-selection-parallel.41 ::annotator 1
# ::snt Aber der kleine Prinz antwortete nicht .
(c0 / contrast-01
    :ARG2 (c3 / antworten-01
              :ARG0 (c1 / Prinz
                        :mod (c2 / kleine))
              :polarity - ))


# ::id 50-selection-parallel.47 ::annotator 1
# ::snt " Es ist bald Zeit zum Frühstücken " , fügte sie kurz darauf hinzu .

(c0 / hinzufügen-02
    :ARG0 (c1 / sie)
    :ARG1 (c5 / denken-01
              :ARG0 c1
              :ARG1 (c7 / zeit
                        :purpose (c4 / frühstücken-01)))
    :time (c2 / darauf
              :mod (c3 / kurz)))

Comment: Translation → “Es ist” to “Ich denke es ist bald Zeit zum Frühstücken” 

English: " I think it is time for breakfast , " she added an instant later .
Spanish: " Creo que es la hora de desayunar ” , añadió ella al instante .


# ::id 50-selection-parallel.50 ::annotator 1
# ::snt Aber er sagte sich : Man weiß ja nie !
(c0 / contrast-01
    :ARG2 (c2 / sagen-01
              :ARG0 (c1 / er)
              :ARG2 c1
              :ARG1 (c3 / wissen-01
                        :polarity -
		 :mod (j / ja)
                        :ARG0 (c6 / man))))

# ::id 50-selection-parallel.50 ::annotator 1
# ::snt Aber er sagte sich : Man weiß ja nie !
(c0 / contrast-01
    :ARG2 (c2 / sagen-01
              :ARG0 (c1 / er)
              :ARG2 c1
	  :ARG1 (b / bestärken-01
		:ARG2 (c3 / wissen-01
                       		:polarity -
                       		:ARG0 (c6 / man)))))


Comment: Translation? “Never” man weiß “nie” plus polarity?
Comment: modal particle ? “Ja” in the sense of bestärken-01?


# ::id 50-selection-parallel.53 ::annotator 1
# ::snt Vulkanische Ausbrüche sind so wie Kaminfeuer .
(c0 / ausbrechen-02
    :ARG1 (c3 / Vulkan)
    :ARG1-of (c2 / ähneln-01
                 :ARG2 (c1 / Kaminfeuer)))

# ::id 50-selection-parallel.56 ::annotator 1
# ::snt Er dachte , er würde nie wiederkommen .
(c0 / denken-01
    :ARG0 (c1 / er)
    :ARG1 (c2 / wiederkommen-01
              :ARG1 c1
              :polarity (c3 / -)
              :time (c4 / nie)))

Comment : “Nie” or “jemals” or nothing - how to prevent double negative

# ::id 50-selection-parallel.59 ::annotator 1
# ::snt Bitte verzeih mir . // I ask your forgiveness .
(c0 / verzeihen-01
    :polite +
    :ARG1 (c1 / ich)
    :mode imperative)

Comment: Translation?

# ::id 50-selection-parallel.62 ::annotator 1
# ::snt Mach dich auf den Weg ! // Now go !
(c0 / machen-01
    :mode imperative
    :ARG0 (c2 / du)
    :location/path (c4 / Weg))

Comment: Preposition drop is in alignment with guidelines, but is this best practice?
Comment: Translation? “Jetzt geh !” - spanish: ¡ Ahora vete !

# ::id 50-selection-parallel.65 ::annotator 1
# ::snt So begann er , ihnen einen Besuch abzustatten, um sich zu beschäftigen und sich schlau zu machen .
(c0 / beginnen-01
    :ARG0 (c1 / er)
    :ARG2 (c2 / besuchen-01
              :ARG0 c1
              :ARG1 (c3 / sie))
    :purpose (c7 / and
                 :ARG1 (c4 / beschäftigen)
                 :ARG2 (c5 / lernen-01)))


Questions:

Re translation (e.g. possession vs. origin; how much “difference” is necessary)

Which is your planet?
Von welchem Planeten bist du denn her?
¿De qué planeta eres?
→ Welcher ist dein Planet? 

# ::id 50-selection-test.9 ::annotator 1
# ::snt Von welchem Planeten bist du denn her ?
(c0 / kommen-01
    :ARG1 (c1 / du)
    :ARG3 (c2 / Planet)
    :domain (c3 / amr-unknown))

# ::id 50-selection-test.9 ::annotator 1
# ::snt Welcher ist dein Planet ?
(c4 / Planet
    :poss (d / du)
    :domain (c6 / amr-unknown))

Now go !
Mach dich auf den Weg !
¡ Ahora vete ! 
Jetzt geh !

# ::id 50-selection-parallel.62 ::annotator 1
# ::snt Mach dich auf den Weg !
(c0 / machen-01
    :mode imperative
    :ARG0 (c2 / du)
    :location (c4 / Weg))

Comment: Preposition drop is in alignment with guidelines, but is this best practice?




Re compounds

see beginning of document^

Re comparative, superlative (align with existing annotation guidlines!)

Es ist viel schwerer, über sich selbst zu richten, als über andere zu urteilen
It is much more difficult to judge oneself than to judge others

(c2 / have-degree-91
   :ARG1 (c1 / richten-01
             :ARG0 (c4 / sich)
             :ARG1 c4)
   :ARG2 (c3 / schwer)
   :ARG3 (c6 / viel)
   :ARG4 (c0 / urteilen-01
             :ARG1 (c5 / andere)))



Verehren bedeutet , zuzugeben , dass ich der schönste , am besten gekleidete , reichste und intelligenteste Mensch auf dem Planeten bin .
To admire means that you regard me as the handsomest , the best - dressed , the richest , and the most intelligent man on this planet . 

(m / bedeuten-01
      :ARG1 (a / verehren-01)
      :ARG2 (z / zugeben-01
            :ARG0 (d / du)
            :ARG1 (i / ich)
            :ARG2 (m6 / Mensch
                  :ARG1-of (h2 / have-degree-91
                        :ARG2 (h / schön)
                        :ARG3 (m2 / am-meisten)
                        :ARG5 (p2 / Planet
                              :mod (d / dem)))
                  :ARG1-of (h3 / have-degree-91
                        :ARG2 (w2 / kleiden)
                        :ARG3 (m3 / am-besten)
                        :ARG5 p2)
                  :ARG1-of (h4 / have-degree-91
                        :ARG2 (r2 / reich)
                        :ARG3 (m4 / am-meisten)
                        :ARG5 p2)
                  :ARG1-of (h5 / have-degree-91
                        :ARG2 (i2 / intelligent
                              :ARG1 m6)
                        :ARG3 (m5 / meisten)
                        :ARG5 p2))))


# ::id lpp_1943.627 ::date 2012-11-26T20:08:34 ::annotator ISI-AMR-05 ::preferred
# ::snt " To admire means that you regard me as the handsomest , the best - dressed , the richest , and the most intelligent man on this planet . "
# ::save-date Wed Sep 20, 2017 ::file lpp_1943_627.txt
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


# ::id lpp_1943.756 ::date 2012-11-13T16:23:34 ::annotator ISI-AMR-05 ::preferred
# ::snt It was the smallest of all .
# ::save-date Mon Sep 18, 2017 ::file lpp_1943_756.txt
(h / have-degree-91
      :ARG1 (i2 / it)
      :ARG2 (s / small)
      :ARG3 (m / most)
      :ARG5 (a / all))




