# Paraphrasing with pre-trained  BART model



#### Introduction

Paraphrasing is the act of expressing something using different words while retaining the original meaning. Let’s see how we can do it with BART, a Sequence-to-Sequence Transformer Model.

Due to time and resource constraints we train our model for only on [Google PAWS-labeled wiki dataset](https://github.com/google-research-datasets/paws#paws-wiki) for two epochs .

#### BART

*BART is a denoising autoencoder for pretraining sequence-to-sequence models. BART is trained by corrupting text with an arbitrary noising function, and  learning a model to reconstruct the original text.*

BART is a denoising autoencoder that maps a corrupted document to the original document it was derived from. [BART ](https://arxiv.org/abs/1910.13461) was released by Facebook on 29th Oct 2019.  It is implemented as a sequence-to-sequence model with a bidirectional encoder over corrupted text and a left-to-right autoregressive decoder. For pre-training, we optimize the negative log likelihood of the original document. BART uses a standard Tranformer-based neural machine translation architecture which, despite its simplicity, can be seen as generalizing BERT (due to the bidirectional encoder), GPT (with the left-to-right decoder), and many other more recent pretraining schemes. 

BART can be fine-tuned to have impressive performances over various tasks like sequence classification (final decoder output connected to a classifier), Sequence Generation (for summarization or question-answering tasks), Machine translation, etc. BART has both an encoder (like BERT) and a decoder (like GPT), essentially getting the best of both worlds. The encoder is a denoising objective similar to BERT while the decoder attempts to reproduce the original sequence (autoencoder), token by token, using the previous (uncorrupted) tokens and the output from the encoder. A significant advantage of this setup is the unlimited flexibility of choosing the corrupting scheme; including changing the length of the original input. 

![p2_BART](README.assets/p2_BART.PNG)

The corruption schemes used in the paper are summarized below:

![image-20210812194423149](README.assets/p2_correction_schemes.PNG)

The authors of BART paper note that training BART with **text infilling** yields the most consistently strong performance across many tasks.

BART is particularly effective when fine tuned for text generation but also works well for comprehension tasks. It matches the performance of RoBERTa with comparable training resources on GLUE and SQuAD, achieves new state of-the-art results on a range of abstractive dialogue, question answering, and summarization tasks, with gains of up to 6 ROUGE. BART also provides a 1.1 BLEU increase over a back-translation system for machine translation, with only target language pretraining. BART with 406M parameters is very close to T5 with 11B parameters in the summarization task. 

#### Training Logs

![p3_training_log](README.assets/p3_training_log.PNG)



#### Sample Results

```
Enter text to paraphrase: A recording of folk songs done for the Columbia society in 1942 was largely arranged by Pjetër Dungu.
Generating outputs:   0%|          | 0/1 [00:00<?, ?it/s]
Original
A recording of folk songs done for the Columbia society in 1942 was largely arranged by Pjetër Dungu.

Predictions >>>
A 1942 recording of folk songs for the Columbia Society was largely arranged by Pjetër Dungu.
A 1942 recording of folk songs for the Columbia Society was largely arranged by Pjetër Dungu.
A 1942 recording of folk songs for the Columbia Society was largely arranged by Pjetër Dungu.
---------------------------------------------------------

Enter text to paraphrase: In mathematical astronomy, his fame is due to the introduction of the astronomical globe, and his early contributions to understanding the movement of the planets.
Generating outputs:   0%|          | 0/1 [00:00<?, ?it/s]
Original
In mathematical astronomy, his fame is due to the introduction of the astronomical globe, and his early contributions to understanding the movement of the planets.

Predictions >>>
In mathematical astronomy, his fame is due to the introduction of the astronomical globe and his early contributions to understanding the movement of the planets.
In mathematical astronomy, his fame is due to the introduction of the astronomical globe and his early contributions to understanding the movement of the planets.
In mathematical astronomy, his fame is due to the introduction of the astronomical globe and his early contributions to understanding the movement of the planets.
---------------------------------------------------------

Enter text to paraphrase: Why are people obsessed with Cara Delevingne?
Generating outputs:   0%|          | 0/1 [00:00<?, ?it/s]
Original
Why are people obsessed with Cara Delevingne?

Predictions >>>
Why are people obsessed with Cara Delevingne?
Why are people obsessed with Cara Delevingne?
Why are people obsessed with Cara Delevingne?
---------------------------------------------------------

Enter text to paraphrase: Earl St Vincent was a British ship that was captured in 1803 and became a French trade man.
Generating outputs:   0%|          | 0/1 [00:00<?, ?it/s]
Original
Earl St Vincent was a British ship that was captured in 1803 and became a French trade man.

Predictions >>>
Earl St Vincent was a British ship captured in 1803 and became a French merchantman.
Earl St Vincent was a British ship captured in 1803 and became a French merchantman.
Earl St Vincent was a British ship that was captured in 1803 and became a French merchantman.
---------------------------------------------------------

Enter text to paraphrase: Worcester is a town and county city of Worcestershire in England.
Generating outputs:   0%|          | 0/1 [00:00<?, ?it/s]
Original
Worcester is a town and county city of Worcestershire in England.

Predictions >>>
Worcester is a town and county town in Worcestershire, England.
Worcester is a town and county town in Worcestershire, England.
Worcester is a town and county town in Worcestershire, England.
---------------------------------------------------------
```

Refer to complete solution [here](https://github.com/krishnarevi/Paraphrasing_with_BART/blob/main/Paraphrasing_with_BART.ipynb) : 
