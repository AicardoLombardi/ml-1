ml
==

##Introduction

This package implement classic machine learning algorithms. Motivations for this project includes:

1. Help machine learning freshman to have a better and deeper understanding of the basic algorithms and model in this field.
2. Provide the `battery-included` code and data for ML prototyping.

Following algorithms and models will be implemented in this package. 
For some of them, `NumPy` support is necessary.
Ensure to configure `NumPy` with advanced linear algebra implementations(`BLAS`, `ATLAS`, `Lapack`). 
The default implementation is extremely slow.

1. Naive Bayes
2. Decision Tree
3. Perceptron
4. Linear Regression
5. Logistic Regression
6. Support Vector Machines
7. AdaBoost
8. Matrix Factorization
9. Neural Network
10. Optimization

##Naive Bayes

nb.py implements `Naive Bayes`(NB) classifier for both `Multi-variate Bernoulli` and `Multinomial` event model.

Run `python nb.py` to train and test NB on `20 Newsgroups` dataset.

By default, train and test data are generated by randomly split the benchmark dataset with an approximate ratio of `4:1`.

Accuracy for `Multi-variate Bernoulli` event model: `78.842676%`

Accuracy for `Multinomial` event model: `84.674503%`

NB is a generative model, different event model leads to different assumption on the distribution of p(x|y).

For example, in spam classification, 
after we decided to generate a spam or non-spam text according to the class prior p(y), 
we need to further decide which words to be included in the text, we can:

1. Go through the whole dictionary, for each word, toss a coin to decide whether to append the word to the text.
2. Randomly select words from the dictionary.

In the first case, each word is drawn from a Bernoulli distribution, 
and the text is generated by the `Multi-variate Bernoulli` event model.

In the second case, all words are drawn from a `Multinomial` distribution.

`TODO`: Parallel NB classifier.

##Decision Tree

dt.py implements `ID3` learning algorithm for training a decision tree(DT).

Run `python dt.py` to train and test ID3 on a subset of the `20 Newsgroups` dataset, 
which contains only 2 news groups: `comp.sys.ibm.pc.hardware.d` and `comp.sys.mac.hardware.e`.

Accuracy for `ID3` on this small dataset is `80.973451%`

`ID3` uses `infomation gain` as the split criteria, and `chi-square test` for early stoping.

It is easy to convert a DT to a rule-set. Check `data/dt.rule_set` for the rule-set learned by DT, like:

<pre>
<code>
    IF (mac) THEN 
      IF (ide) THEN 
        PREDICT LABEL IS comp.sys.ibm.pc.hardware.d [branch-size : 8]
      ELSE 
        IF (controller) THEN 
          PREDICT LABEL IS comp.sys.mac.hardware.e [branch-size : 5]
        ELSE 
          IF (difference) THEN 
            IF (people) THEN 
              PREDICT LABEL IS comp.sys.ibm.pc.hardware.d [branch-size : 2]
            ELSE 
              PREDICT LABEL IS comp.sys.mac.hardware.e [branch-size : 13]
          ELSE 
            PREDICT LABEL IS comp.sys.mac.hardware.e [branch-size : 156]
</code>
</pre>


##Perceptron

perceptron.py implements `Perceptron Learning Algorithm`(PLA) and its variant `Pocket` for binary classification.

Run `python perceptron.py` to train and test PLA and `Pocket` on `heart-scale` dataset.

Accuracy for Perceptron: `75.471698%`

Accuracy for `Pocket` : `81.132075%`

PLA is one of the simplest machine learning algorithm: 
whenever a mistake happens, if the model misclassify a positive sample as negative, PLA adds the x to the w;
otherwise, substract it from w.

`Pocket` is the upgrade version of PLA: it always keep a copy of the best model after each update, 
and this is how the name `Pocket` come from.

PLA and `Pocket` are very old-fashioned ml technics, but they are very important, 
they are the foundations of Support Vector Machines and Neural Network, 
and the weighted version of `Pocket` will be used as the weak classifier of `AdaBoost`.

##Matrix Factorization

recsys/mf.py implements `matrix factorization`(MF) algorithms for recommendation.

Run `python recsys/mf.py` to train and test MF on the `1 million` version of `MovieLens` dataset.

RMSE on the test data is `0.932806` after `5 iterations`.

MF is optimized by `Stochastic Gradient Descent`(SGD) algorithm in mini-batch manner. 
By combining `NumPy` with mini-batch, the training algorithm is very efficient with an affordable memory cost.

##Hidden Markov Model

hmm.py implements `Hidden Markov Models`(HMMs) for solving `sequence labeling` problem.

Run `python hmm.py` to train and test POS-tagger using trigram HMMs on a small corpus.

HMMs is trained by `Maximum Likelihood Estimated`(MLE), and smooth technics is necessary for good performance.

Using HMMs for labeling sequence is a little difficult. The brute force solution is not sufficient enough, 
thus a `Dynamic Programming`(DP) algorithm is used here: the famous `Viterbi` algorithm.

For comparasion, a baseline tagger is also implemented by ignore the state transition probability. 

Accuracy for baseline tagger: `91.661447%`

Accuracy for `Viterbi` tagger: `93.490334%`

<pre><code>
P--V-----V-----T---V----D--------N-------I----N---I---P----N---C------N-----`-----N-----P----N-------N-----.-'-
|  |     |     |   |    |        |       |    |   |   |    |   |      |     |     |     |    |       |     | | 
I was appalled to read the misstatements of facts in your Oct. 13 editorial `` Colombia 's Brave Publisher . ''
</code></pre>





