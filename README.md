# Automatic Continuation of User-Generated Item Lists 

### Introduction

This is the implementation of the paper as well as datasets and their splitting used in the paper:<br>
> Consistency-Aware Recommendation for User-Generated ItemList Continuation.<br>
> WSDM, 2020.<br>
> Yun He, Yin Zhang, Weiwen Liu and James Caverlee.<br>
> Department of Computer Science and Engineering, Texas A&M University.<br>
> Contact: yunhe@tamu.edu <br>
> Personal Page: http://people.tamu.edu/~yunhe/ <br>

User-generated item lists are popular on many platforms. Examples include song-based playlists on Spotify, image-based lists (or“boards”) on Pinterest, book-based lists on Goodreads, and answer-based lists on question-answer forums like Zhihu.
In these platforms, user-generated item lists are manually created, curated, and managed by users. 
Typically, users must first identify candidate items, determine if they are a good fit for a list, add them to a list, and then potentially provide ongoing updates
to the list (e.g., by adding or deleting items over time). To accelerate this process and assist users to explore more related itemsfor their lists, 
we study the important yet challenging problem of user-generated item list continuation. That is, how can we recommend items that are related to the list and fit the user’s preferences?

### Usage 
We propose a novel model CAR to predict the next item that a user will possibly add into a list.

#### Input
The input of CAR includes: (1) the containing relationship between lists and items; (2) the creating relationship between users and lists.

#### (1) The containing relationship between lists and items.
``listId itemId``

which means that those items are curated in this list. These interactions are stored in \data folder, like AotM.txt.zip (unzip this file when you run our code). 

Note that the last item is regarded as test data, the item before the last item is regarded as validation data and the rest of items are treated as training data.

#### (2) The containing relationship between lists and items.
The format of train.txt, validation.txt and test.txt are like this:

``userId listId 1.0``

And items contained in that list.

``listId [itemId_1, itemId_2, itemId_3,...]``

which means that the list contains these items. These containing relationships are storded in listItem_\*.txt, like AttList/AttList_cikm2019/data/goodreads/listItem_goodreads.txt

#### Output
The output are the evaluation results comparing the ranked lists of AttList and the groundtruth from the test.txt. An example is presented as follows:

```
precision@5: 0.0297358342335
recall@5: 0.0993438722178
precision@10: 0.0212903859373
recall@10: 0.139846678794
NDCG@5: 0.0738304666949
NDCG@10: 0.088376425288
```

#### Run
python Attlist_cikm2019.py --dataSetName Spotify

For more hyper-parameters, please see Attlist_cikm2019.py.

### Files in folder

#### attList_CIKM2019_data
- -goodreads.zip, unpreprocessed goodreads dataset;
- -spotify.zip, unpreprocessed spotify dataset;
- -zhihu.zip, unpreprocessed zhihu dataset;
- -examples_listContainItem.txt;
- -examples_userFollowingList.txt;

#### AttList_cikm2019
- -Attlist_cikm2019.py, the main function of AttList;
- -AttLayer_cikm2019.py, the file of vanilla attention module;
- -AttLayerSelf_cikm2019.py, the file of self-attention module;
- -getDataSet_cikm2019.py, the file to get data structure for training and testing;
- -evaluate_cikm2019.py, the file to do the evaluation;

#### Data (included in AttList_cikm2019)
- -goodreads: userList_goodreads.txt, listItem_goodreads.txt, train.txt, validation.txt and text.txt;
- -spotify: userList_spotify.txt, listItem_spotify.txt, train.txt, validation.txt and text.txt;
- -zhihu: userList_zhihu.txt, listItem_zhihu.txt, train.txt, validation.txt and text.txt;

### Citation
@inproceedings{he2019hierarchical,<br>
  title={A Hierarchical Self-Attentive Model for Recommending User-Generated Item Lists},<br>
  author={He, Yun and Wang, Jianling and Niu, Wei and Caverlee, James},<br>
  booktitle={Proceedings of the 28th ACM International Conference on Information and Knowledge Management},<br>
  pages={1481--1490},<br>
  year={2019},<br>
  organization={ACM}<br>
}<br>

### Important Tip
The evaluation protocol in our paper is to generated the prediction scores for all lists in the dataset, which is slow. Hence, in the future, we may first randomly sample 100 negative samples. And the model only needs to predict scores for these 100 negative samples and ground-truth samples. This protocol has been widely used in recommendation research community.
