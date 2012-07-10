README 
------
(created 3/30/12, last updated 7/10/12, by Ferhan Ture, fture@cs.umd.edu)

Mapping from Wikipedia internal docids to docnos used in our system.

`dewiki_docno-mapping.dat` = mapping from German Wikipedia internal docids to our docnos (to use this, see class edu.umd.cloud9.collection.DocnoMapping in our [Cloud9 code library]: http://lintool.github.com/Cloud9)
enwiki_docno-mapping.dat = mapping from English Wikipedia internal docids to our docnos
dewiki_docno-mapping.txt = same as dewiki_docno-mapping.dat, in text format.
enwiki_docno-mapping.txt = same as enwiki_docno-mapping.dat, in text format.

( format: [Wikipedia docid]\t[docno] )

```
$ head -3 enwiki_docno-mapping.txt
12	1
25	2
39	3
```
---------------------------------

### Similar German-English document pairs found by the cross-lingual pwsim algorithm (and ground truth pairs), as described in Ture, Lin and Elsayed, SIGIR 2011.

sample.docnos = list of docnos for the 1064 sample German articles used in evaluations

$ head -3 sample.docnos 
508
4077
6033

---------------------------------

d1000_q300_t400_b2000.pwsim-all = all docno pairs, found simiar by the cross-lingual pwsim algo, with parameters #bits=1000, #tables=300, hamming threshold=400, windowsize=2000
d1000_q300_t400_b2000.pwsim-sample = docno pairs corresponding to the 1064 sampled German docnos

( format: [English docno]\t[German docno]\t[Hamming distance] )

$ head -3 d1000_q300_t400_b2000.pwsim-sample
2	77357	391
7	49806	399
41	1049339	396

----------------------------------

ground_t30.cosine = docnos of all English articles corresponding to any of the 1064 German sample articles, as found by brute-force over docvectors, cosine threshold = 0.3
ground_t30_top1.cosine = same as above, but keep top 1 for each German article

( format: [English docno]\t[German docno]\t[Cosine similarity] )

$ head -3 ground_t30.cosine 
3012458	11366	0.332
8205	11366	0.326
492411	11366	0.325
$ head -3 ground_t30_top1.cosine 
3012458	11366	0.332
739791	14456	0.380
1318820	18551	0.324

---------------------------------

interwiki-links.en2de = docnos of all English articles corresponding to any of the 1064 German sample articles, as suggested by Wikipedia interwiki links (at most 1 for each German article)

( format: [English docno]\t[German docno] )

$ head -3 interwiki-links.en2de 
1534	299154
5708	121894
7845	64857

----------------------------------

### Bitext extracted from German and English Wikipedia, using the algorithm described in Ture and Lin, NAACL-HLT 2012.
### This bitext was the best performing in our experiments (see paper), where we used the two-stage classification approach: 
### Threshold of 0.98 was applied on simple classifier in first stage (output = 13,746,173 pairs), and a 0.60 threshold was applied on the complex classifier in the second stage (final output = 5,761,517 pairs). 

bitext-wiki_de-en.de.bz2 = German side of the bitext, lowercased, no tokenization, bz2-compressed. 
bitext-wiki_de-en.en.bz2 = English side of the bitext, lowercased, no tokenization, bz2-compressed.

$ bzcat bitext-wiki_de-en.de.bz2 | tail -3
kategorie:chief minister (orissa)
saddam hussein unterschrieb als vizepräsident persönlich einen zusicherungsvertrag  
saddam hussein unterschrieb als vizepräsident persönlich einen zusicherungsvertrag  

$ bzcat bitext-wiki_de-en.en.bz2 | tail -3
sitalsasthi  (may–june, orissa, neighbouring regions)	
saddam hussein declared that the invasion was a response to it.	
on december 30, 2006, saddam hussein was hanged.

--------------------------------- 

### Second version of German-English bitext, after a series of changes to our bitext extraction code, and with casing preserved.
### This bitext has not been evaluated on MT training yet.
### Threshold of 0.98 was applied on simple classifier in first stage (output = 16,139,076 pairs), and a 0.62 threshold was applied on the complex classifier in the second stage (final output = 5,404,313 pairs).

bitext-wiki-v2_de-en.de.bz2 = German side of the bitext, in raw format (no tokenization or lowercasing).
bitext-wiki-v2_de-en.en.bz2 = English side of the bitext, in raw format (no tokenization or lowercasing).

$ bzgrep 'Hitler' bitext-wiki-v2_de-en.de.bz2 | head -4
(1939) The High Cost of Hitler
(Deutsch: Hitlers Bankier.
(siehe Adolf Hitler:Politische Anfänge).
1. Januar 1925 von Hitler privat als persönlicher Mitarbeiter angestellt, war Schaub als einer von Hitlers persönlichen Adjutanten bis ins Jahr 1945 ständig in Hitlers Nähe.

$ bzgrep 'Hitler' bitext-wiki-v2_de-en.en.bz2 | head -4
(1939) The High Cost of Hitler		
Hitler's Banker: Hjalmar Horace Greeley Schacht.		
see Adolf Hitler for more books		
On January 1, 1925 hired privately by Hitler as a personal assistant, Schaub was one of Hitler's personal adjutant to the year 1945 and in constant close to Hitler.		

