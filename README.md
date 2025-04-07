# README

## 1. Transform and pre-process the dataset.

Run:

```bash
python3 transform-dataset/json2csv.py
```

to transform the dataset to json files to user-readable csv files.

## 2. Embedding generation and clustering.

First, playlists titles and tracks are embedded using a pre-trained SentenceBERT model and stored in /embeddings/embeddings.pkl:

```bash
python3 clustering-no-split/embeddings/track_embeddings_no-split.py
```

Then, the K-means clustering algorithm is applied to create the clusters, and the generated csv file (in `/clustering-no-split/clusters/200`) is modified to calculate and include the percentage of exact matches:

```bash
python3 clustering-no-split/clusters/clustering-no-split.py clustering-no-split/clusters/percent-no-split.py
```

The resulted csv file is stored into clustering-no-split/clusters/200/clusters_with_exact_matches.csv

Apply the clean algorithm to remove miscellaenous clusters and save the output in clustering-no-split/clean/200:

```bash
python3 clustering-no-split/clean/clean.py
```

Finally, randomly split the clusters:

```bash
python3 clustering-no-split/split/split.py
```

## 3. Finetuning
 
Run:

```bash
python3 finetuning/cross_entropy_model_finetuning.py finetuning/finetuning_triplet_loss.py
```

to train the sentenceBERT model using respectively the cross-entropy loss and the triplet loss.

## 4. Generate the embeddings for playlists titles using the fine-tuned models.

Tun :

```bash
python3 embeddings/playlists_embeddings_final.py
```

to save the embeddings generated by the fine-tuned model to a pickle file. Make sure to adjust the model path to select either the triplet loss model, the cross-entropy loss model or the pretrained model.

## 5. Generate the recommendations and evaluate the models.

Evaluate the metrics for a given test playlist:

```bash
python3 similarity/test_1_playlist_finetuned_model.py
```

Generate the recommendation for a playlist title:

```bash
python3 similarity/recommend.py
```

Evaluate the metrics on the whole test set:
```bash
python3 similarity/testset_test_model.py
```
Make sure to adjust the model path to select either the triplet loss model, the cross-entropy loss model or the pretrained model.
