# Building a spellchecker for Ligurian verbs

This repository contains a minimal spellchecker for the present indicative tense of Ligurian verbs belonging to the first conjugation (infinitive ending in _-â_).

## First conjugation basics

In the most general case, Ligurian regular verb conjugations display apophony. Some inflected forms have a vowel change compared to the base form of the infinitive. We see an example here for the verb _portâ_ “to carry”, which alternates between the theme _pòrt-_ [ˈpɔːrt-] (which we may call **stressed prefix**) and _port-_ [purt-] (the **unstressed prefix**).

| Tense      | Forms          |
|------------|----------------|
| inf.       | portâ          |
| past part. | m: portou      |
|            | f: portâ       |
|            | pl: portæ      |
| gerund     | portando       |
| pres. ind. | 1s: pòrto      |
|            | 2s: pòrti      |
|            | 3s: pòrta      |
|            | 1p: portemmo   |
|            | 2p: portæ      |
|            | 3p: pòrtan     |

Often, verb conjugations may not have any apophony, as is the case for _lavâ_ (“to wash”). We can consider such verbs to be a special case, where the two prefixes (stressed and unstressed) are homographs:

| Tense      | Forms          |
|------------|----------------|
| inf.       | lavâ           |
| past part. | m: lavou       |
|            | f: lavâ        |
|            | pl: lavæ       |
| gerund     | lavando        |
| pres. ind. | 1s: lavo       |
|            | 2s: lavi       |
|            | 3s: lava       |
|            | 1p: lavemmo    |
|            | 2p: lavæ       |
|            | 3p: lavan      |

Much [like in Italian](https://en.wikipedia.org/wiki/Italian_orthography#C_and_G), the letters ‹c› and ‹g› represent [tʃ] and [dʒ] respectively when they precede ‹i› or ‹e›, but otherwise they represent [k] and [ɡ].

In order to represent [tʃ] and [dʒ] in front of vowel letters other than ‹i› or ‹e›, one inserts an ‹i› which in this case will not represent any sound, but simply alters the nature of the preceding consonant. We therefore have e.g. _canta_ [ˈkaŋta] “he/she sings” → _cianta_ [ˈtʃaŋta] “plant”. Conversely, if we want to achieve the sounds [k] or [ɡ] in front of ‹i› or ‹e›, we must insert the silent letter ‹h›, which turns what would have been [tʃ] and [dʒ] into [k] and [ɡ] respectively. We have e.g. _gia_ [ˈdʒiːa] “he/she turns” → _ghia_ [ˈɡiːa] “guide”.

Sometimes, in verb conjugations, “silent” ‹h› and ‹i› have to be inserted. Consider the conjugation of _pagâ_ “to pay”:

| Tense      | Forms          |
|------------|----------------|
| inf.       | pagâ           |
| past part. | m: pagou       |
|            | f: pagâ        |
|            | pl: pagæ       |
| gerund     | pagando        |
| pres. ind. | 1s: pago       |
|            | 2s: paghi      |
|            | 3s: paga       |
|            | 1p: paghemmo   |
|            | 2p: pagæ       |
|            | 3p: pagan      |

Note how we have _paghemmo_ [paˈɡemˑu] and not \*_pagemmo_ [paˈdʒemˑu], as well as _paghi_ [ˈpaːɡi] and not \*_pagi_ [ˈpaːdʒi]. The ‹h› gets inserted to maintain the -[ɡ]- sound in front of ‹e› and ‹i›.

Conversely, observe the conjugation for _mangiâ_ [manˈdʒaː] “to eat”, where the ‹i› is “silent”, i.e. its only purpose is to represent -[dʒ]- rather than -[ɡ]-:

| Tense      | Forms          |
|------------|----------------|
| inf.       | mangiâ         |
| past part. | m: mangiou     |
|            | f: mangiâ      |
|            | pl: mangiæ     |
| gerund     | mangiando      |
| pres. ind. | 1s: mangio     |
|            | 2s: mangi      |
|            | 3s: mangia     |
|            | 1p: mangemmo   |
|            | 2p: mangiæ     |
|            | 3p: mangian    |

Note how we have _mangemmo_ [maŋˈdʒemˑu] and not \*_mangiemmo_: since ‹g› has a “soft” sound [dʒ] in front of ‹e›, the “silent” ‹i› is dropped. Note also how we have _mangi_ and not \*_mangii_.

There are however cases where the _i_ is not silent, such as _giâ_ “to turn” [dʒiˈaː] ~ [ˈdʒjaː]:

| Tense      | Forms          |
|------------|----------------|
| inf.       | giâ            |
| past part. | m: giou        |
|            | f: giâ         |
|            | pl: giæ        |
| gerund     | giando         |
| pres. ind. | 1s: gio        |
|            | 2s: gii        |
|            | 3s: gia        |
|            | 1p: giemmo     |
|            | 2p: giæ        |
|            | 3p: gian       |

Note in this case how we have _gii_ [ˈdʒiː] and _giemmo_ [dʒiˈemˑu] ~ [ˈdʒjemˑu], and not \*_gi_ [ˈdʒi] and \*_gemmo_ [ˈdʒemˑu].

## Spellchecker

In order to build a spellchecker for a moderately inflected language like Ligurian, it would of course be completely infeasible to list out every possible form of every possible regular verb. Instead, the [Hunspell](https://hunspell.github.io/) software, which we will be using, allows us to define some inflection rules.

The files `lij.aff` and `lij.dic` define a basic spellchecker for the present indicative tense of _portâ_, _lavâ_, _pagâ_ and _mangiâ_. The format of these files is described in [hunspell’s section 4 `man` page](https://manpages.ubuntu.com/manpages/trusty/en/man4/hunspell.4.html).

Look at the `.aff` file. Ignoring for the time being the `TRY`, `MAP` and `REP` directives, you will want to focus on the `SFX` directives, which define suffixation rules to produce inflected forms of the verbs starting from the stressed prefix (the `aa` rules) and the unstressed prefix (the `AA` rules). The `.dic` file is where we tell hunspell what to apply these rules to, with lines such as `pòrto/aa` (= “apply stressed suffixation rules to _pòrto_”).

Have a look at the `man` page linked above to see if you can understand the syntax of these rules.

Once you’ve installed `hunspell` (on Mac, with [homebrew](https://brew.sh/): `brew install hunspell`), you can verify that this dictionary produces the right forms using the `unmunch` command, which generates all possible forms:
```
$ unmunch lij.dic lij.aff
…
pòrto
pòrti
pòrta
pòrtan
portemmo
portæ
lavo
lavi
lava
lavan
lavemmo
lavæ
pago
paghi
paga
pagan
paghemmo
pagæ
mangio
mangi
mangia
mangian
mangemmo
mangiæ
```

## Next steps

The examples above only describe the present indicative of first conjugation verbs. The full conjugation tables for the first conjugation of the four verbs discussed can be found [here](https://docs.google.com/spreadsheets/d/1PGxrbxBAUlcno2BpDsC1vKtuowbwu5QCGOOfzSg9m78/edit#gid=0).

Can you extend the `.aff` file to produce the full conjugation?

Can you also deal with the case of _giâ_? Remember: the ‹i› there is not silent! You may want to define a new set of suffixation rules for cases with a non-silent ‹i›, since whether ‹i› is silent or not cannot simply be determined by the spelling of the word.
