# [AN@PHORA](https://github.com/max-ionov/russian-anaphora/)
source code [here](https://github.com/max-ionov/russian-anaphora/)
## Requirements

 To reanimate the code as is on Ubuntu you will probably need the latest version of [FreeLing](http://nlp.lsi.upc.edu/freeling/index.php/node/8), an open source language analysis tool suite, and antiquated DS tools:

```
python2.7 
numpy==1.16.6
scipy==0.14
sklearn==0.14
```


## FreeLing server

In order to resolve your anaphora you will apparently need the server up and running. To do that you may use this [manual](https://github.com/TALP-UPC/FreeLing-User-Manual/blob/master/docs/analyzer.md) but be careful starting the server: the stated there command 

```analyze -f ru.cfg --server --port 50005 &  ```

as described in the manual runs the server that returns data unusable for our goals. To run any of our scripts we need to have words lemmatized and tagged, thus we need to run the server with additional argument *--outlv tagged* from supposedly */usr/local/bin*:

```analyze -f ru.cfg --server --outlv tagged --port 50005 &```

To check the server's performance you may run 

```echo "Я надел носки, которые лежали на полу. Они красные." | analyzer_client 50005```

The correct output is supposed to be:

```
Я я ENS0000 0.996729
надел надевать VDSMS0F0000 0.98507
носки носка NCFPFI0000 0.319725
, , Fc 1
которые который RNP000 0.739195
лежали лежать VDP0S0N0A00 1
на на B0 0.999321
полу пол NCLSMI0000 0.596431
. . Fp 1

Они они ENP0000 1
красные красный ANP00F000 0.962839
. . Fp 1

```

The corrupt output may seem like:

```Я я JJ 0.117669
надел надел NN 0.336089
носки носки NN 0.336089
, , Fc 1
которые которые JJ 0.117669
лежали лежали NN 0.336089
на на NN 0.336089
полу полу NN 0.336089
. . Fp 1

Они они NP00SP0 1
красные красные VBD 0.0727811
. . Fp 1

```

## Anaphora resolution

**Note** that the paths to FreeLing components should strictly correspond to those in the python code.

All done, we can run python scripts that help us to solve Anaphora resolution problem.

Execution of 

``` 
echo 'Я надел носки, которые лежали на полу. Они красные.' | ./resolute-text.py - config.txt model.rf.all.dat

```
gives us

```
freeling ready
communication ok
***
которые  --->  носки
Я надел носки, носки лежали на полу. Они красные.

Они  --->  носки
Я надел носки, носки лежали на полу. носки красные.

```

Another script
```
echo 'Я надел носки, которые лежали на полу. Они красные.' | ./anaphora.py 
```
has the following output:
```
Я надел носки, которые лежали на полу. Они красные.
freeling ready
communication ok
носки	<---	которые		relative
носки	<---	Они		pronoun
Anaphoric expressions: 2
```
## TODO
The following step is to convert the scripts into python3
