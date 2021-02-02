## Try Solving [OpenFst Examples](http://www.openfst.org/twiki/bin/view/FST/FstExamples)

# Tokenization

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms >Mars.fst <<EOF
0 1 M Mars
1 2 a <epsilon>
2 3 r <epsilon>
3 4 s <epsilon>
4
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait Mars.fst | dot -Tjpg >Mars.jpg
```

![Mars.fst](./Mars.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms >Martian.fst <<EOF
0 1 M Martian
1 2 a <epsilon>
2 3 r <epsilon>
3 4 t <epsilon>
4 5 i <epsilon>
5 6 a <epsilon>
6 7 n <epsilon>
7
EOF
```

![Martian.fst](./Martian.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms >man.fst <<EOF
0 1 m man
1 2 a <epsilon>
2 3 n <epsilon>
3
EOF
```

![man.fst](./man.jpg)

```
fstunion man.fst Mars.fst | fstunion - Martian.fst | fstclosure >lexicon.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait lexicon.fst | dot -Tjpg -Gdpi=150 >lexicon.jpg
```

![lexicon.fst](./lexicon.jpg)

```
fstrmepsilon lexicon.fst | fstdeterminize | fstminimize >lexicon_opt.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait lexicon_opt.fst | dot -Tjpg >lexicon_opt.jpg
```

![lexicon_opt.fst](./lexicon_opt.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms >punct.fst <<EOF
0 1 <space> <epsilon>
0 1 . <epsilon>
0 1 , <epsilon>
0 1 ? <epsilon>
0 1 ! <epsilon>
1
EOF
```

![punct.fst](./punct.jpg)

```
fstunion man.fst Mars.fst | fstunion - Martian.fst | fstconcat - punct.fst | fstclosure >lexicon_punc.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait lexicon_punc.fst | dot -Tjpg -Gdpi=200>lexicon_punc.jpg
```

![punct.fst](./lexicon_punc.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms >Marsman.fst <<EOF
0 1 M M
1 2 a a
2 3 r r
3 4 s s
4 5 <space> <space>
5 6 m m
6 7 a a
7 8 n n
8 9 ! !
9
EOF
```
```
fstdraw --isymbols=ascii.syms --osymbols=ascii.syms -portrait Marsman.fst | dot -Tjpg >Marsman.jpg
```

![Marsman.fst](./Marsman.jpg)

