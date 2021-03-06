### **This is work in progress**

# GLUES: General Language Understanding Evaluation in Spanish

The GLUES Benchmark aims to collect different sources of tasks for evaluating Spanish Language Models in a unified fashion in order to develop and allow the growth of the Spanish NLP Community.


# Table of Contents
1. [Tasks](#tasks)
2. [Statistics](#statistics)
3. [Baselines](#baselines)
<!-- 4. [Optional](#optional) -->
4. [References](#references)


# Tasks
In this section we present a set of tasks that we think will allow for consistent evaluation of future spanish models. This compilation was made possible in part thanks to the efforts of the comunity for the development of cross-lingual datasets. This has allowed us to use the spanish portions of these dataset in order to assert a spanish-only model performance.

Part of the motivation for this work was comparing a spanish-only model to the performance of a multilingual model like Multilingual BERT. Given this, many of the tasks were chose to perform relevant comparisons of our model's performance to the findings from Shijie Wu et al. [\[1\]][1], so some of the decisions made for this compilations were made in order to allow for fair comparison against their results.


### Natural Language Inference
**XNLI** 

The Cross-Lingual NLI Corpus [\[2\]][2] is a evaluation dataset that extends the MNLI [\[3\]][3] dataset by adding dev and test set for 15 languages. Given a premise sentence and a hypothesis sentences, the task is to predict whether the premise entails the hypothesis (entailment), contradicts the hypothesis (contradiction), or neither (neutral). 

In this setup we train using the Spanish portion of the MNLI dataset, and use the dev and test set from the XNLI corpus. This task is evaluated by simple accuracy.


### Paraphrasing

**PAWS-X**

The PAWS-X[\[4\]][4] is the multilingual version of the PAWS dataset [\[5\]][5]. The task consists in determining if two sentences are semantically equivalent or not.

The dataset provides standard (translate) train, dev and test set. It is evaluated using simple accuracy.


### Named Entity Recognition

**CoNLL-2002 Shared Task**

Named Entity Recognition consists in determining if each word in a sentence corresponds to an entity or not. Named entitie are phrases that contain the names of persons, organizations, locations, times and quantities. This particular dataset focuses on the first three and adds a fourth category of miscellaneous entities. This dataset is tagged using the `BIO` schema, which differentiates between the beginning of an entity, inside of an entity or outside. The following example show how the `BIO` schema works together with the four categories previously mentioned:
```
Wolff		B-PER
,		O
currently	O
a		O
journalist	O
in		O
Argentina	B-LOC
,		O
played		O
with		O
Del		B-PER
Bosque		I-PER
```

The dataset[\[6\]][6] is presented as one word per line followed with its respective entity tag. An empty line represents the end of a sentence. This dataset provides standar train, dev and test sets and the performance in this task is measured with F1 rate. For this task, precision is the percentage of named entities found that are correct and recall is the percentage of named entities present in the corpus that are found.  


### Part-of-Speech Tagging (_missing confirmation for dataset selection_)
<!-- * UDv1.4 [\[4\]][4], following tthe work from [\[5\]][5]
* accuracy
 -->
**Universal Dependencies v1.4**

Part-of-speech tagging (POS tagging) is the task of tagging a word in a text with its part of speech. A part of speech is a category of words with similar grammatical properties. Common Spanish parts of speech are noun, verb, adjective, adverb, pronoun, preposition, conjunction, etc.

For this task we use the spanish subset of the Universal Dependencies (v1.4) Treebank [\[7\]][7]. Since there are two spanish subsets to this dataset, we form the final dataset by concatenating btoh. The version of the dataset was chosen following the works of Shijie Wu et al. [\[1\]][1] and  Kim et al. [\[8\]][8].

The dataset provides standard train, dev and test sets. This task is evaluated by the accuracy of predicted POS tags.


### Dependency Parsing
<!-- * UDv2.2 [\[6\]][6], since it was the version for the CoNLL 2017 Shared Task [\[7\]][7]
* [UAS, LAS](https://linguistics.stackexchange.com/a/6895)
 -->
**Universal Dependencies v2.2**

The task of dependency parsing consists in assigning a dependecy tree to a given sentence. A dependency tree represents the grammatical structure of a sentence and defines the relationship between "head" words an "dependent" words which are associated to those heads. The relationship between the two words is expressed with an edge of the dependency tree and the type of relationship is represented by the label of said edge.

For this task we use a subset of the Universal Dependencies v2.2 Treebank [\[9\]][9]. The spanish portion of this dataset consists of three subsets, `Spanish_AnCora`, `Spanish_GSD` and `Spanish_PUD`. We use the concatenation of the `AnCora` and `GSD` portions of the dataset. This desition and the version choice was done following the work from Ahmad et al. [\[10\]][10]

This task is evaluated using the metrics UAS and LAS, which stand for Unlabeled Attachment Score and Labeled Attachment Score, respectively. UAS is the percentage of words that have been assigned the correct head, whereas LAS is the percentage of words that have been assigned both the correct head and the correct label for the relationship.

### Document Classification (comments ??)
<!-- * MLDoc [\[8\]][8]
* accuracy
 -->
**MLDoc**

The MLDoc [\[11\]][11] dataset is a balanced subset of the Reuters corpus [\[12\]][12]. This task consists in classifying the docuements into four categories, CCAT (Corporate/Industrial), ECAT (Economics), GCAT (Government/Social), and MCAT (Markets).

This dataset provides multiple sizes for the train split (1k, 2k, 5k and 10k), plus standard dev and test sets. We chose to train using the largest available test split. This task is evaluated using simple accuracy.


# Statistics

The following table shows the number of examples in each split of each dataset, the type of the task and the metrics used to evaluate the task.

For each task the meaning of an "example" are different. For XNLI, PAWS-X and Dependency Parsing an example is a sentence that needs to be processed whereas in POS Tagging and NER Tagging, each word is an example that needs to be classified. Finally, examples in the task of Document Classification are whole documents. The metrics reported correspond to each task's example definition.

<!-- Original rows of the table -->
<!-- | UDv1.4     | 389,703 + 463,275 | 41,975 + 54,665 | 8,128 + 54,755 | POS            | acc      | -->
<!-- | UDv2.2     | 14,187 + 14,305 | 1,400 + 1,654 | 426 + 1,721 | parsing        | UAS, LAS | -->


| Corpus                            | \|Train\| | \|Dev\| | \|Test\| | Task           | Metrics  |
|-----------------------------------|-----------|---------|----------|----------------|----------|
| XNLI                              | 392,703   | 2,490   | 5,010    | NLI            | acc      |
| PAWS-X                            | 49,401    | 1,962   | 1,999    | paraphrase     | acc      |
| CoNLL-2002                        | 273,037   | 54,837  | 53,049   | NER            | f1 score |
| UDv1.4                            | 852,978   | 96,640  | 62,883   | POS            | acc      |
| UDv2.2 <sup>[1](#footnote1)</sup> | 28,492    | 3,054   | 2,147    | parsing        | UAS, LAS |
| MLDoc                             | 10,000    | 1,000   | 4,000    | classification | acc      |

<a name="footnote1">1</a>: This dataset was obtained by concatenating the two larges spanish subsets.

# Baselines
In this section we present a series of baseline results for each task. Considering some of the tasks have a long running history in the field, the original baselines have long been surpassed by modern architectures and methods. Nonetheless, we think it is important to cite the original baselines for completeness.

Given that our main goal is to motivate and standardize the growth of spanish models from now on, we also sumarize the current state of the art for each task, so that future efforts have a current baseline to compare to.

### Original Baselines

| Corpus     | Task           | Performance                                   |
|------------|----------------|-----------------------------------------------|
| XNLI       | NLI            | 68.8 [\[2\]][2]                               |
| PAWS-X     | paraphrase     | 89.3 [\[4\]][4]                               |
| CoNLL-2002 | NER            | 35.86 [\[6\]][6]                              |
| UDv1.4     | POS            | - <sup>[2](#footnote2)</sup>                  |
| UDv2.2     | parsing        | 90.10/87.55 <sup>[3](#footnote3)</sup> [\[13\]][13] |
| MLDoc      | classification | 94.45 [\[11\]][11]                            |

<a name="footnote2">2</a>: This is a standard task for which we didn't find an original baseline.<br>
<a name="footnote3">3</a>: Measured only on the AnCora portion of the dataset.

### Current state of the art

| Corpus     | Task           | Performance          |
|------------|----------------|----------------------|
| XNLI       | NLI            | 80.80 [\[14\]][14]   |
| PAWS-X     | paraphrase     | 89.0 [\[4\]][4]      |
| CoNLL-2002 | NER            | 88.81 [\[15\]][15]   |
| UDv1.4     | POS            | 98.91 [\[16\]][16]   |
| UDv2.2     | parsing        | 92.3/86.5 [\[1\]][1] |
| MLDoc      | classification | 94.45 [\[11\]][11]   |

<!-- # Optional
(_opinions on this????_)
This might not interest everyone but it has a special place in our hearts

### Document Classification
💕💕💕
* ArgumentMining2017 (multiple tasks) [\[13\]][13]
* top-5 accuracy, macro-averaged precision, recall and f1 (multiple tasks)
 -->

# References
<!-- I used this weird way of adding hyperlinks to be able to link to same url from different places -->
1. [Beto, Bentz, Becas: The Surprising Cross-Lingual Effectiveness of BERT][1]
2. [XNLI: Evaluating Cross-lingual Sentence Representations][2]
3. [A Broad-Coverage Challenge Corpus for Sentence Understanding through Inference][3]
4. [PAWS-X: A Cross-lingual Adversarial Dataset for Paraphrase Identification][4]
5. [PAWS: Paraphrase Adversaries from Word Scrambling][5]
6. [Introduction to the CoNLL-2002 Shared Task: Language-Independent Named Entity Recognition][6]
7. [Universal Dependencies v1.4][7]
8. [Cross-Lingual Transfer Learning for POS Tagging without Cross-Lingual Resources][8]
9. [Universal Dependencies v2.2][9]
10. [On Difficulties of Cross-Lingual Transfer with Order Differences: A Case Study on Dependency Parsing][10]
11. [A Corpus for Multilingual Document Classification in Eight Languages][11]
12. [Reuters Corpora (RCV1, RCV2, TRC2)][12]
13. [CoNLL 2017 Shared Task][13]
14. [Cross-lingual Language Model Pretraining][14]
15. [Neural Architectures for Nested NER through Linearization][15]
16. [75 Languages, 1 Model: Parsing Universal Dependencies Universally][16]
<!-- Optional -->
<!-- 13. [200K+ Crowdsourced Political Arguments for a New Chilean Constitution][13] -->


[1]: https://arxiv.org/abs/1904.09077
[2]: https://arxiv.org/abs/1809.05053
[3]: https://www.nyu.edu/projects/bowman/multinli/paper.pdf
[4]: https://arxiv.org/abs/1908.11828
[5]: https://arxiv.org/abs/1904.01130
[6]: https://www.aclweb.org/anthology/W02-2024/
[7]: https://lindat.mff.cuni.cz/repository/xmlui/handle/11234/1-1827
[8]: https://www.aclweb.org/anthology/D17-1302/
[9]: https://lindat.mff.cuni.cz/repository/xmlui/handle/11234/1-2837
[10]: https://arxiv.org/abs/1811.00570
[11]: http://www.lrec-conf.org/proceedings/lrec2018/pdf/658.pdf
[12]: https://trec.nist.gov/data/reuters/reuters.html
<!-- [13]: https://www.aclweb.org/anthology/W17-5101/ -->
[13]: http://universaldependencies.org/conll17/baseline.html

[14]: https://arxiv.org/abs/1901.07291
[15]: https://arxiv.org/abs/1908.06926
[16]: https://arxiv.org/abs/1904.02099