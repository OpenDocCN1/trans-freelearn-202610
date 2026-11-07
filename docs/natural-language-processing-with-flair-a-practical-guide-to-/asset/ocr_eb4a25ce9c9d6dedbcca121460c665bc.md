```
nlayers=1)

trainer = LanguageModelTrainer(language_model, corpus)
trainer.train('forward_model_directory',
    sequence_length=25,
    mini_batch_size=10,
    max_epochs=100)
```

In the first part of the preceding code snippet, we initialize the LanguageModel class by telling it that we are building a forward model with a single layer and a hidden size of 64. hidden_size is an important parameter that defines the size of the LSTM nodes' hidden size, which in effect defines the dimensionality of our embedding vectors. hidden_size would generally need to be set much higher, to a number such as 2,048. But, because we are training a model on a language with roughly 120 words and we are doing this only as part of an exercise, we can get away with low-dimensional embeddings.

In the second part of the previous snippet, we start the model training process by initializing the LanguageModelTrainer class and telling it to use a sequence length of 25 and run it for 100 epochs. The parameters chosen here are purposely much lower than the parameters that would have been used if we were training a large embedding model for production. In practice, it's a good idea to use longer sequence lengths than this and do more epochs.

We then start training by calling LanguageModelTrainer.train() where we specify the output directory of the trained model, which in our case is forward_model_directory/. This directory will be created relative to the directory we are running this code from.

If we did everything right, the model training should commence, and Flair should start spitting out some debug text and metric values to standard output:

```
ms/batch  5.88 | loss  0.77 | ppl    2.15
Epoch    97: reducing learning rate of group 0 to 1.2207e-03.
best loss so far  0.98
('\n ni li sike. mi pini e ma mije? kala ni li sin e pan lawa
en kena t
---------------------------------------------------------------
end of split 1/1 | epoch  97 | time:  1.75s | valid loss 0.99
---------------------------------------------------------------
Epoch time: 1.92
read text file with 1 lines
```