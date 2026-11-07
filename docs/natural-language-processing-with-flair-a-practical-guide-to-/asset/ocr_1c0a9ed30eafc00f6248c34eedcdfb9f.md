+   - PRT: Particles or other function words
+   - X: Other: foreign words, typos, abbreviations
+   - .: Punctuation

The preceding tags are less English-specific and can be used across multiple languages. They are used for Flair's multilingual PoS taggers (described in more detail in the following section).

# Types of PoS taggers in Flair

Flair ships with a number of pretrained PoS taggers. Just like NER taggers, Flair's PoS taggers can be classified as either monolingual or multilingual taggers.

Monolingual pretrained PoS taggers in Flair are trained on a single language and often use language-specific tag sets.

Some of the commonly used taggers are described as follows:

+   - English PoS tagger trained on the `Ontonotes` dataset using the Penn tag set.
+     ID: `pos`
+   - A faster, lighter, and less accurate version of the English PoS tagger trained on the `Ontonotes` dataset using the Penn tag set.
+     ID: `pos-fast`
+   - English PoS tagger trained on using the Universal POS tag set.
+     ID: `upos`
+   - A faster, lighter, and less accurate version of the `upos` tagger using the Universal POS tag set.
+     ID: `upos-fast`
+   - German PoS tagger trained on the `UD German - HDT` dataset.
+     ID: `de-pos`

The preceding taggers will achieve optimal performance on the language they were trained on, but will not work well on any other language. To deal with multilingual corpora, Flair offers a set of multilingual PoS taggers. They were trained on 12 languages: English, German, French, Italian, Dutch, Polish, Spanish, Swedish, Danish, Norwegian, Finnish, and Czech. To load them, simply use the `pos-multi` or `pos-multi-fast` tagger IDs.