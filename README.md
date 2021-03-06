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
fstdraw --isymbols=ascii.syms --osymbols=ascii.syms -portrait Marsman.fst | dot -Tjpg -Gdpi=150>Marsman.jpg
```

![Marsman.fst](./Marsman.jpg)

```
fstcompose Marsman.fst lexicon_punc.fst | fstproject --project_output | fstrmepsilon >tokens.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait tokens.fst | dot -Tjpg >tokens.jpg
```

![tokens.fst](./tokens.jpg)

# Exercise 1

(a) Create a transducer that maps numbers in the range 0 - 999999 represented as digit strings to their English read form, e.g.:

```
1 -> one
11 -> eleven
111 -> one hundred eleven
1111 -> one thousand one hundred eleven
11111 -> eleven thousand one hundred eleven
```

for one one game:
```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms >1.fst <<EOF
0 1 1 one
1
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait 1.fst | dot -Tjpg >1.jpg
```

![1.fst](./1.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms >11.fst <<EOF
0 1 1 eleven
1 2 1 <epsilon>
2
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait 11.fst | dot -Tjpg >11.jpg
```

![11.fst](./11.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms >111.fst <<EOF
0 1 1 one
1 2 <epsilon> hundred
2 3 <epsilon> eleven
3 4 1 <epsilon>
4 5 1 <epsilon>
5
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait 111.fst | dot -Tjpg >111.jpg
```

![111.fst](./111.jpg)


```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms >1111.fst <<EOF
0 1 1 one
1 2 <epsilon> thousand
2 3 <epsilon> one
3 4 <epsilon> hundred
4 5 <epsilon> eleven
5 6 1 <epsilon>
6 7 1 <epsilon>
7 8 1 <epsilon>
8
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait 1111.fst | dot -Tjpg -Gdpi=150>1111.jpg
```

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms >11111.fst <<EOF
0 1 1 eleven
1 2 <epsilon> thousand
2 3 <epsilon> one
3 4 <epsilon> hundred
4 5 <epsilon> eleven
5 6 1 <epsilon>
6 7 1 <epsilon>
7 8 1 <epsilon>
8 9 1 <epsilon>
9
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait 11111.fst | dot -Tjpg -Gdpi=150>11111.jpg
```

```
fstunion 1.fst 11.fst | fstunion - 111.fst | fstunion - 1111.fst | fstunion - 11111.fst | fstconcat - punct.fst | fstclosure > 5m1.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait 5m1.fst | dot -Tjpg -Gdpi=300>5m1.jpg
```

![5m1.fst](./5m1.jpg)


```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms >input1.fst <<EOF
0 1 1 1
1 2 1 1
2 3 1 1
3 4 1 1
4 5 <space> <space>
5 6 1 1
6 7 1 1
7 8 1 1
8 9 , ,
9 10 1 1
10 11 . .
11
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=ascii.syms -portrait input1.fst | dot -Tjpg -Gdpi=150>input1.jpg
```

![input1.fst](./input1.jpg)

```
fstcompose input1.fst 5m1.fst | fstproject --project_output | fstrmepsilon > out1.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait out1.fst | dot -Tjpg -Gdpi=150>out1.jpg
```

![out1.fst](./out1.jpg)


for range 0 - 999999 :

add to wotw.syms

```
seventeen   7103
eighteen    7104
nineteen    7105
sixty   7106
seventy 7107
eighty  7108
```

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > is_digit.fst << EOF
0 1 1 1
0 1 2 2
0 1 3 3
0 1 4 4
0 1 5 5
0 1 6 6
0 1 7 7
0 1 8 8
0 1 9 9
0 1 0 0
1
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=ascii.syms -portrait is_digit.fst | dot -Tjpg >is_digit.jpg
```

![is_digit.fst](./is_digit.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms > 0.fst << EOF
0 1 0 <epsilon>
1
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait 0.fst | dot -Tjpg >0.jpg
```

![0.fst](./0.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms > name0.fst << EOF
0 1 1 one
0 1 2 two
0 1 3 three
0 1 4 four
0 1 5 five
0 1 6 six
0 1 7 seven
0 1 8 eight
0 1 9 nine
1
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name0.fst | dot -Tjpg >name0.jpg
```

![name0.fst](./name0.jpg)

test 1 digit(<10):
```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > test7.fst <<EOF
0 1 7 7
1
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=ascii.syms -portrait test7.fst | dot -Tjpg >test7.jpg
```

```
fstcompose test7.fst name0.fst | fstproject --project_output | fstrmepsilon > test7_out.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait test7_out.fst | dot -Tjpg >test7_out.jpg
```

for 2 digits(10<=,<20):
```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms > name1.fst << EOF
0 1 1 <epsilon>
1 2 0 ten
1 2 1 eleven
1 2 2 twelve
1 2 3 thirteen
1 2 4 fourteen
1 2 5 fifteen
1 2 6 sixteen
1 2 7 seventeen
1 2 8 eighteen
1 2 9 nineteen
2
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name1.fst | dot -Tjpg >name1.jpg
```

![name1.fst](./name1.jpg)

testing 2 digits(10<=,>20):

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > test15.fst << EOF
0 1 1 1
1 2 5 5
2
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=ascii.syms -portrait test15.fst | dot -Tjpg >test15.jpg
```

```
fstcompose test15.fst name1.fst | fstproject --project_output | fstrmepsilon > test15_out.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait test15_out.fst | dot -Tjpg >test15_out.jpg
```

for 2 digits(0,>20):

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms > name2pre.fst << EOF
0 1 0 <epsilon>
0 1 2 twenty
0 1 3 thirty
0 1 4 forty
0 1 5 fifty
0 1 6 sixty
0 1 7 seventy
0 1 8 eighty
0 1 9 ninety
1
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name2pre.fst | dot -Tjpg >name2pre.jpg
```

![name2pre.fst](./name2pre.jpg)

```
fstunion name0.fst 0.fst | fstconcat name2pre.fst - | fstunion - name1.fst > name2.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name2.fst | dot -Tjpg -Gdpi=150>name2.jpg
```

![name2.fst](./name2.jpg)

```
fstrmepsilon name2.fst | fstdeterminize | fstminimize >name2_opt.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name2_opt.fst | dot -Tjpg >name2_opt.jpg
```

![name2_opt.fst](./name2_opt.jpg)

testing 2 digits:

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > test35.fst <<EOF
0 1 3 3
1 2 5 5
2
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=ascii.syms -portrait test35.fst | dot -Tjpg >test35.jpg
```

![test35.fst](./test35.jpg)

```
fstcompose test35.fst name2_opt.fst | fstproject --project_output | fstrmepsilon > test35_out.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait test35_out.fst | dot -Tjpg >test35_out.jpg
```

![test35_out.fst](./test35_out.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > test50.fst <<EOF
0 1 5 5
1 2 0 0
2
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=ascii.syms -portrait test50.fst | dot -Tjpg >test50.jpg
```

![test50.fst](./test50.jpg)

```
fstcompose test50.fst name2_opt.fst | fstproject --project_output | fstrmepsilon > test50_out.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait test50_out.fst | dot -Tjpg >test50_out.jpg
```

![test50_out.fst](./test50_out.jpg)

for 3 digits:

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms > hundred.fst << EOF
0 1 <epsilon> hundred
1
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait hundred.fst | dot -Tjpg >hundred.jpg
```

![hundred.fst](./hundred.jpg)

```
fstconcat name0.fst hundred.fst |fstunion - 0.fst | fstconcat - name2.fst > name3.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name3.fst | dot -Tjpg -Gdpi=150>name3.jpg
```

![name3.fst](./name3.jpg)

```
fstrmepsilon name3.fst | fstdeterminize | fstminimize >name3_opt.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name3_opt.fst | dot -Tjpg -Gdpi=150>name3_opt.jpg
```

![name3_opt.fst](./name3_opt.jpg)

testing 3 digits:

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > test234.fst << EOF
0 1 2 2
1 2 3 3
2 3 4 4
3
EOF
```

```
fstcompose test234.fst name3_opt.fst | fstproject --project_output | fstrmepsilon > test234_out.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait test234_out.fst | dot -Tjpg >test234_out.jpg
```

![test234_out.fst](./test234_out.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > test200.fst << EOF
0 1 2 2
1 2 0 0
2 3 0 0
3
EOF
```

```
fstcompose test200.fst name3_opt.fst | fstproject --project_output | fstrmepsilon > test200_out.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait test200_out.fst | dot -Tjpg >test200_out.jpg
```

![test200_out.fst](./test200_out.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > test204.fst << EOF
0 1 2 2
1 2 0 0
2 3 4 4
3
EOF
```

```
fstcompose test204.fst name3_opt.fst | fstproject --project_output | fstrmepsilon > test204_out.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait test204_out.fst | dot -Tjpg >test204_out.jpg
```

![test204_out.fst](./test204_out.jpg)

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > test040.fst << EOF
0 1 0 0
1 2 4 4
2 3 0 0
3
EOF
```

```
fstcompose test040.fst name3_opt.fst | fstproject --project_output | fstrmepsilon > test040_out.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait test040_out.fst | dot -Tjpg >test040_out.jpg
```

![test040_out.fst](./test040_out.jpg)

for 4 digits:

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms > thousand.fst << EOF
0 1 <epsilon> thousand
1
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait thousand.fst | dot -Tjpg >thousand.jpg
```

![thousand.fst](./thousand.jpg)

```
fstconcat name0.fst thousand.fst | fstunion - 0.fst | fstconcat - name3.fst > name4.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name4.fst | dot -Tjpg -Gdpi=250>name4.jpg
```

![name4.fst](./name4.jpg)

```
fstrmepsilon name4.fst | fstdeterminize | fstminimize > name4_opt.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name4_opt.fst | dot -Tjpg -Gdpi=250>name4_opt.jpg
```

![name4_opt.fst](./name4_opt.jpg)

testing 4 digits:

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > test3467.fst << EOF
0 1 3 3
1 2 4 4
2 3 6 6
3 4 7 7
4
EOF
```

```
fstcompose test3467.fst name4_opt.fst | fstproject --project_output | fstrmepsilon > test3467_out.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait test3467_out.fst | dot -Tjpg -Gdpi=150>test3467_out.jpg
```

![test3467_out.fst](./test3467_out.jpg)

for 5 digits:

```
fstunion name1.fst name2.fst | fstconcat - thousand.fst | fstconcat - name3.fst > name5.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name5.fst | dot -Tjpg -Gdpi=400>name5.jpg
```

![name5.fst](./name5.jpg)

```
fstrmepsilon name5.fst | fstdeterminize | fstminimize > name5_opt.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name5_opt.fst | dot -Tjpg -Gdpi=150>name5_opt.jpg
```

![name5_opt.fst](./name5_opt.jpg)

testing 5 digits:

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > test46789.fst << EOF
0 1 4 4
1 2 6 6
2 3 7 7
3 4 8 8
4 5 9 9
5
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=ascii.syms -portrait test46789.fst | dot -Tjpg >test46789.jpg
```

![test46789.fst](./test46789.jpg)

```
fstcompose test46789.fst name5_opt.fst | fstproject --project_output | fstrmepsilon > test46789_out.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait test46789_out.fst | dot -Tjpg -Gdpi=150>test46789_out.jpg
```

![test46789_out.fst](./test46789_out.jpg)


for 6 digits:

```
fstconcat name3.fst thousand.fst | fstconcat - name3.fst > name6.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name6.fst | dot -Tjpg -Gdpi=550>name6.jpg
```

![name6.fst](./name6.jpg)

```
fstrmepsilon name6.fst | fstdeterminize | fstminimize > name6_opt.fst
```

```
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait name6_opt.fst | dot -Tjpg -Gdpi=250>name6_opt.jpg
```

![name6_opt.fst](./name6_opt.jpg)

testing 6 digits:

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > test467890.fst << EOF
0 1 4 4
1 2 6 6
2 3 7 7
3 4 8 8
4 5 9 9
5 6 0 0
6
EOF
```

```
fstdraw --isymbols=ascii.syms --osymbols=ascii.syms -portrait test467890.fst | dot -Tjpg >test467890.jpg
```

![test467890.fst](./test467890.jpg)

```
fstcompose test467890.fst name6_opt.fst | fstproject --project_output | fstrmepsilon > test467890_out.fst
```

```
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait test467890_out.fst | dot -Tjpg -Gdpi=150>test467890_out.jpg
```

![test467890_out.fst](./test467890_out.jpg)

for (0 - 999999):

```
fstunion name0.fst name1.fst | fstrmepsilon | fstdeterminize | fstminimize > name01.fst
fstunion name01.fst name2.fst | fstrmepsilon | fstdeterminize | fstminimize > name012.fst
fstunion name012.fst name3.fst | fstrmepsilon | fstdeterminize | fstminimize > name0123.fst
fstunion name0123.fst name4.fst | fstrmepsilon | fstdeterminize | fstminimize > name01234.fst
fstunion name01234.fst name5.fst | fstrmepsilon | fstdeterminize | fstminimize > name012345.fst
fstunion name012345.fst name6.fst | fstrmepsilon | fstdeterminize | fstminimize > final.fst
```

testing (0 - 999999):

```
fstcompose test7.fst final.fst | fstproject --project_output | fstrmepsilon > final7.fst
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait final7.fst | dot -Tjpg>final7.jpg
```

![final7.fst](./final7.jpg)

```
fstcompose test35.fst final.fst | fstproject --project_output | fstrmepsilon > final35.fst
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait final35.fst | dot -Tjpg>final35.jpg
```

![final35.fst](./final35.jpg)

```
fstcompose test50.fst final.fst | fstproject --project_output | fstrmepsilon > final50.fst
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait final50.fst | dot -Tjpg>final50.jpg
```

![final50.fst](./final50.jpg)

```
fstcompose test200.fst final.fst | fstproject --project_output | fstrmepsilon > final200.fst
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait final200.fst | dot -Tjpg>final200.jpg
```

![final200.fst](./final200.jpg)

```
fstcompose test204.fst final.fst | fstproject --project_output | fstrmepsilon > final204.fst
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait final204.fst | dot -Tjpg>final204.jpg
```

![final204.fst](./final204.jpg)

```
fstcompose test234.fst final.fst | fstproject --project_output | fstrmepsilon > final234.fst
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait final234.fst | dot -Tjpg>final234.jpg
```

![final234.fst](./final234.jpg)

```
fstcompose test3467.fst final.fst | fstproject --project_output | fstrmepsilon > final3467.fst
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait final3467.fst | dot -Tjpg -Gdpi=150>final3467.jpg
```

![final3467.fst](./final3467.jpg)

```
fstcompose test46789.fst final.fst | fstproject --project_output | fstrmepsilon > final46789.fst
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait final46789.fst | dot -Tjpg -Gdpi=150>final46789.jpg
```

![final46789.fst](./final46789.jpg)

```
fstcompose test467890.fst final.fst | fstproject --project_output | fstrmepsilon > final467890.fst
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait final467890.fst | dot -Tjpg -Gdpi=200>final467890.jpg
```

![final467890.fst](./final467890.jpg)

(b) Incorporate this transduction into the letter-to-token transduction above and apply to the input Mars is 4225 miles across. represented as letters.

```
fstcompile --isymbols=ascii.syms --osymbols=wotw.syms > is.fst << EOF
0 1 i is
1 2 s <epsilon>
2
EOF

fstcompile --isymbols=ascii.syms --osymbols=wotw.syms > miles.fst << EOF
0 1 m miles
1 2 i <epsilon>
2 3 l <epsilon>
3 4 e <epsilon>
4 5 s <epsilon>
5
EOF

fstcompile --isymbols=ascii.syms --osymbols=wotw.syms > across.fst << EOF
0 1 a across
1 2 c <epsilon>
2 3 r <epsilon>
3 4 o <epsilon>
4 5 s <epsilon>
5 6 s <epsilon>
6
EOF

fstunion Mars.fst is.fst | fstunion - miles.fst | fstunion - across.fst >words.fst
fstrmepsilon words.fst | fstdeterminize | fstminimize >words_opt.fst
fstdraw --isymbols=ascii.syms --osymbols=wotw.syms -portrait words_opt.fst | dot -Tjpg -Gdpi=200>words_opt.jpg
```
![words_opt.fst](./words_opt.jpg)

```
fstunion words_opt.fst final.fst | fstconcat - punct.fst | fstclosure > exe1.fst
```

```
fstcompile --isymbols=ascii.syms --osymbols=ascii.syms > input.fst << EOF
0 1 M M
1 2 a a
2 3 r r
3 4 s s
4 5 <space> <space>
5 6 i i
6 7 s s
7 8 <space> <space>
8 9 4 4
9 10 2 2
10 11 2 2
11 12 5 5
12 13 <space> <space>
13 14 m m
14 15 i i
15 16 l l
16 17 e e
17 18 s s
18 19 <space> <space>
19 20 a a
20 21 c c
21 22 r r
22 23 o o
23 24 s s
24 25 s s
25 26 . .
26
EOF

fstdraw --isymbols=ascii.syms --osymbols=ascii.syms -portrait input.fst | dot -Tjpg -Gdpi=300>input.jpg
```

![input.fst](./input.jpg)

```
fstcompose input.fst exe1.fst | fstproject --project_output  | fstrmepsilon >Mars_is_4225_miles_across.fst
fstdraw --isymbols=wotw.syms --osymbols=wotw.syms -portrait Mars_is_4225_miles_across.fst | dot -Tjpg -Gdpi=200>Mars_is_4225_miles_across.jpg
```

![Mars_is_4225_miles_across.fst](./Mars_is_4225_miles_across.jpg)

