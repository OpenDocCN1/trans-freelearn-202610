# BOW representations (BOW and SAX-VSM)

The BOW approach applies a sliding window over a time series and can transform each subsequence into a word using **symbolic aggregate approximation (SAX)**. SAX reduces a time series into a string of arbitrary lengths. It's a two-step process that starts with a dimensionality reduction via **piecewise aggregate approximation (PAA)** and then a discretization of the obtained simplified time series into SAX symbols. You can leverage the `pyts.bag_of_words.BagOfWords` module to build the following representation:

![](bbox=[0.12, 0.27, 0.88, 0.48])

Figure 1.23 – Example of SAX to transform a time series

The BOW approach can be modified to leverage **term frequency-inverse document frequency (TF-IDF)** statistics. TF-IDF is a statistic that reflects how important a given word is in a corpus. SAX-VSM (SAX in vector space model) uses this statistic to link it to the number of times a symbol appears in a time series. You can leverage the `pyts.classification.SAXVSM` module to build this representation.

# Bag of SFA Symbols (BOSS and BOSS VS)

The BOSS representation uses the structure-based representation of the BOW method but replaces PAA/SAX with a **symbolic Fourier approximation (SFA)**. It is also possible to build upon the BOSS model and combine it with the TF-IDF model. As in the case of SAX-VSM, BOSS VS uses this statistic to link it to the number of times a symbol appears in a time series. You can leverage the `pyts.classification.BOSSVS` module to build this representation.