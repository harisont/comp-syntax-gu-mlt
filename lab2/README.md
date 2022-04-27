# Lab 2: Multilingual generation and translation

This lab corresponds to Chapters 5 to 9 of the Notes, but follows them only loosely.
Therefore we will structure it according to the exercise sessions
rather than chapters.

The abstract syntax is given in the subdirectory `grammar/abstract/`

The compulsory lab assignment has two alternatives:

1. Complete MiniLang for your language of choice
2. Complete MicroLang + application grammar (Doctor) for your language of choice

The following instructions refer to the latter alternative. If you want to work on the Mini grammar you can ignore everything about the Doctor grammar and use the Micro grammar instructions while working on the corresponding Mini files.

See [below](#a-method-for-testing-your-micro-and-mini-grammar) to read about testing your grammar(s).

## Session 5

1. Design a morphology for the main lexical types (N, A, V) with parameters and a couple of paradigms.
2. Test it by implementing the lexicon. You need to define lincat N,A,V,V2 as well as the paradigms in MicroResource (see testing instructions [below](#testing-the-morphology)).

*To deliver*: the lexicon part of files MicroGrammarX.gf and MicroResourceX.gf for your language of choice X (if you are working with the Mini grammar, there is also a dedicated MiniLexicon module). Follow the structure of the files under `english` when preparing these.

## Session 6

1. Define the linearization types of main phrasal categories - the remaining categories in MicroLang.
2. Define the rest of the linearization rules in MicroLang.

*To deliver*: MicroLangX and MicroResourceX for your language of choice, with the lexicon part from Session 5 completed with syntax part. 

## Session 7

1. Complete the rest of the definitions of RGL categories and functions in MiniGrammar.
2. Add concrete UD labels.
3. Generate a synthetic UD treebank. The simplest way to do this from the
GF shell is random generation; change english/MiniLangEng.labels to
the labels of your own language:

```
gr -cat=Utt -number=22 -depth=6 | vd -output=conll -abslabels=abstract/MiniLang.labels -cnclabels=english/MiniLangEng.labels
```

*Hint*: if your language is already in the standard RGL, you can implement it easily by making a copy of the files in grammar/swedish.
But use this implementation only as a reference with which to test your own one: your lab submission must not make any use of the RGL modules, except Prelude!

*To deliver* (but only if you select this assignment): file Mini..X.gf for your language X, together with MiniLangX.labels and a treebank in CoNLL format with 20 trees.

*Deadline*: until the end of the course.

## Session 8

1. Implement a simple application grammar, application/Doctor.gf.
This can be be done either by using your own MiniResource or the standard resource.
There are two versions of English implementation to support this.
Your first approximation can be just to copy it and change the words.

*To deliver* (but only if you select this assignment): file DoctorX.gf for your language X.

*Deadline*: until the end of the course.


## Testing your Micro and Mini grammar

### Testing the morphology
To check if a lexical item inflects correctly, you can:
1. import the MicroLang module with `-retain` option
2. run `cc` (`compute_concrete`, see the [GF shell reference](https://www.grammaticalframework.org/doc/gf-shell-reference.html) for explanations) on the lexical item

Example from the GF shell:

```
> i -retain MicroLangEng.gf
- compiling /home/harisont/Repos/harisont/aprendendo-GF/comp-syn-lab2-alt2/abstract/MicroLang.gf...   write file /home/harisont/Repos/harisont/aprendendo-GF/comp-syn-lab2-alt2/abstract/MicroLang.gfo
- compiling MicroLangEng.gf...   write file MicroLangEng.gfo
22 msec
> cc travel_V 
{s = table
       MicroResEng.VForm
       ["travel"; "travels"; "traveled"; "traveled"; "traveling"];
 lock_V = <>}
0 msec
```

For a more readable output, you can use the `-table` or `-list` option:

```
> cc -table travel_V 
s . MicroResEng.Inf => travel
s . MicroResEng.PresSg3 => travels
s . MicroResEng.Past => traveled
s . MicroResEng.PastPart => traveled
s . MicroResEng.PresPart => traveling
0 msec
> cc -list travel_V 
travel, travels, traveled, traveled, traveling
0 msec
```
### Comparison with a reference implementation
Since MicroLang is a proper part of the RGL, it can be easily implemented as an application grammar.
How to do this is shown in `grammar/functor/`, where the implementation consists of two files:
- `MicroLangFunctor.gf` which is a generic implementation working for all RGL languages,
- `MicroLangFunctorEng.gf` which is a *functor instantiation* for English, easily reproduciple for other languages than `Eng`.

To use this for testing, you can take the following steps:

1. Build a functor instantiation for your language by copying `MicroLangFunctorEng.gf` and changing `Eng` in the file name and inside the file to your language code.

2. Use GF to create a testfile by random generation:
```
  $ echo "gr -number=1000 | l -tabtreebank" | gf english/MicroLangEng.gf functor/MicroLangFunctorEng.gf >test.tmp
```

3. Inspect the resulting file `test.tmp`.
But you can also use Unix `cut` to create separate files for the two versions of the grammar and `diff` to compare them:
```
  $ cut -f2 test.tmp >test1.tmp
  $ cut -f3 test.tmp >test2.tmp
  $ diff test1.tmp test2.tmp

  52c52
  < the hot fire teachs her
  ---
  > the hot fire teaches her
  69c69
  < the man teachs the apples
  ---
  > the man teaches the apples
  122c122
  ```
As seen from the result in this case, our implementation has a wrong inflection of the verb "teach".

The Mini grammar can be tested in the same way, by building a reference implementation using the functor in `functor/`.





