Not bad! :)

I found some formatting/numbering/invalid labels errors that make it impossible to process the file with various automatic tools. For example:

- forgotten underscores
- invalid dependency labels (e.g. `adv` instead of `advmod`)
- underscores in place of dependency labels: if you were unsure what label to use, better to use `dep`. I did non remember that, but the `DEPLABEL` field cannot be left empty
- wrong numbering (skipped some indices)

However, these are not a big deal and you can easily spot them with a validator such as the one available [here](https://github.com/UniversalDependencies/tools), using the option `--level 2`.

The actual errors I could find are: 

English: 

- sent. 14: I would say that "prison" refers to "years", not to "penalty"
- sent. 15: there is a copula, so you should link subject to its complement "carried", not to the main (aux) verb "was"
- sent. 18: `conj` does not work in that way. From other sentences I see you understand how it should be used, but here is an (automatically obtained) tree that is closer to being right, just for comparison: 

    ```conllu
    # sent_id = 18
    # text = the casings had rotted away and had to be replaced
    1	the	the	DET	DEF	Definite=Def|PronType=Art	2	det	_	_
    2	casings	casing	NOUN	PL-NOM	Number=Plur	4	nsubj	_	_
    3	had	have	AUX	PAST-AUX	Mood=Ind|Tense=Past|VerbForm=Fin	4	aux	_	_
    4	rotted	rot	VERB	PERF	Mood=Ind|Tense=Past|VerbForm=Fin	0	root	_	_
    5	away	away	ADV	_	_	4	advmod	_	_
    6	and	and	CCONJ	_	_	10	cc	_	_
    7	had	have	AUX	PAST-AUX	Mood=Ind|Tense=Past|VerbForm=Fin	10	aux	_	_
    8	to	to	PART	_	_	10	mark	_	_
    9	be	be	AUX	INF	VerbForm=Inf	10	aux:pass	_	_
    10	replaced	replace	VERB	PASS	Tense=Past|VerbForm=Part|Voice=Pass	4	conj	_	_
    ```

- sent. 19: good analysis, but I think "in front of" should be treated as `fixed` multiword expression
- sent. 21: "atropine and autoinjectors" is the object of "carry" not of "troops" (but I'm sure this was just out of distraction)

But most importantly, __your first few "English" sentences are in Swedish__. This is the one thing it is important that you fix before resubmitting.

Swedish:
- sent. 7: I'd say "över hela världen" refers to the main verb "åkte", not to "sa"
- sent. 10: tokens are not in the correct order
- sent. 11: I think that the root of the subtree "eller borde vara det" is "det", which stands for "bekant", and not the verb "vara", which is used as copula
- sent. 13: "och" should be a dependent of the `conj` bytte. In most cases, you annotated conjunctions correctly, but not always.
- sent. 19: I think "över att denna flicka skulle våga göra henne till åtlöje inför hela klassen" refers to "upprörd", not to "blev"
- sent. 21: "i" should not refer to "för" (probably a typo)
