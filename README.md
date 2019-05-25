# wikiconflict

This repository is part of the analysis done for my master thesis.

## Research Goal
 Thesis aims to identify the grpup of edited tokens in the revisin history of an article who are fine grained in there respective revisions.
 
In order to achieve our reserch goal we break it into following steps.

1. We define and identify fine grained edit tokens, we call it Change Objects.
2. We transform Change Objects to Change Vector of fixed dimension using pre-trained word vectors.
3. We create groups of Change Objects by clustering change vector.
4. We evaluate groups of fine grained Change Objects.
5. We compare our algorithm of identifying  group of Change Objects with algorithm proposed by [bykau et al].

All the analysis steps are relased as IPython notebook.

Following steps needed to be performed for processing of the data.


## Prepearing the code.
First clone or download this repository in a folder.
 follow the following step to rerun the analysis

### 1. Download the Pre-Trained word vectors
After cloning this repository get the word vectors from [fast text](https://github.com/facebookresearch/fastText/blob/master/docs/pretrained-vectors.md). Download [English Word Vector] [https://dl.fbaipublicfiles.com/fasttext/vectors-wiki/wiki.en.vec] *Warning: word vector is a huge file*. Create a directory /wordvectors in the root directory where the code is cloned. Store the word vector in this directory to be used in next steps.

### 2. Creating required directory for storing the intermediate and final outcome of analysis.

Inside wikiconflict directory create the directory /data for storing all the results of analysis.
Inside /data create various subdirectories. Each of these subdirectories will store data at various stage of processing. 
1. /content
2. /change objects
3. /change_vector
4. /bykau_change_object
5. /annotation

## Steps of analysis

### 1.Downalod the Article
We use tokens from [WikiWho API]() to identify edited tokens, so first step requirs to downlaod all the content of the article. 
Tokenised content of the article can be downloaded using the [notebook](./notebooks/1_download_rev_content.ipynb) which is saved in the data/content directory for next steps of analysis.

### 2. Ceate Change Object

From the edited tokens downlaoded in the data/content directory we create change vector using the using the notebook [2_create_change_object-v2.ipynb](./notebooks/2_create_change_object-v2.ipynb). This notebook saves the identified change object in the directory   data/change objects

### Create Change Vector

Next step is to transform Change Objects stored in /data/change objects into 600 dimensional Change Vector using pre trained word vectors downloaded from [fast text](https://github.com/facebookresearch/fastText/blob/master/docs/pretrained-vectors.md). 
 The notebook  [3_create_change_vector-v3.ipynb](./notebooks/[3_create_change_vector-v3.ipynb]) creates different change vectors corresponding to different values of parameter, *context_length*. All of these change vectors are saved in data/change_vector directory. Change vector is created using neighbouring tokens of change vectors and *context_length* equals to number of tokens in left and right used to create change vectors. Corresponsing to different values of *context_length* we get different Change Vectors for same Change Object.

### Cluster and Evaluate
Change Vectors saved in data/change_vector corresponnding to different values of *context_legth* us used create DBSCAN cluster. These clusters are first evaluated using both intrinsic and extrinsic mechanism. Finally we compare our cluster to one created by re-implementation of Bykau. et. al. DBSCAN has two parameters eps and min_samples which when combined with context_length gives us three paramters of our model against which we evaluate our model.



#### Intrinsic Evaluation
 
 Being a density based clustering algorithm DBSCAN identifies clusters of unequal size. We first analyse the cluster length distribution using various descriptive statistics. We also identify Gini co effecient of cluster length distribution.

For further investaigating Chnage object groups Intrinsicially, we propose various measure based on the assumption that cluster of vector created by averaging word tokens from immediately before and after the Change Object should be able to have similar word tokens and come from similar relative position in the article.  In order to quantify the different kind of words in Cluster we define token entropy for the edited token in gap of change object. Similarly to qunatify the relative position of Change Objects in a cluster we define the reltive position entropy. All of the intrinsic evaluation analysis is done in the notebook [4_1_clustering-dbscan-intrinsic-evaluation-all.ipynb](./notebooks/4_1_clustering-dbscan-intrinsic-evaluation-all.ipynb). 

#### Extrinsic Evaluation

We extriniscally evaluate our model against a golden data set prepared human annotator. We use entropy based measure of Homogenity,Completness and V-measure. Extrinsic evaluation is done in the [4_2_clustering-dbscan-extrinsic-evaluation-v-measure.ipynb](./notebooks/4_2_clustering-dbscan-extrinsic-evaluation-v-measure.ipynb)

#### Compare with Bykau. et. Al.

First we reimplement paper from Bykau. et. al. using 5_1_reproduce_fine_grained.ipynb. We save the change object created by Bykau. et. Al. in /bykau_change_object. Then we cluster these change object and compare against change object generated by our model.

####  Agreement of our cluster with Bykau. et. Al.
Using we create clusters for all 16 articles in [small article list](https://github.com/acifer/wikiconflict/blob/master/conflicted_article.csv) and [big article list](https://github.com/acifer/wikiconflict/blob/master/conflicted_article-big.csv)

#### Extrinsic evaluation against Bykau. et. Al.




