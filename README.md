# C-MORE
Code for the ACL2022 paper "[C-MORE: Pretraining to Answer Open-Domain Questions by Consulting Millions of References]()"

```bib
@inproceedings{yue2022cmore,
 title={C-MORE: Pretraining to Answer Open-Domain Questions by Consulting Millions of References},
 author={Xiang Yue and Xiaoman Pan and Wenlin Yao and Dian Yu and Dong Yu and Jianshu Chen},
 booktitle={ACL},
 year={2022}
}
```

## Data
C-MORE contains ~3M question-context pairs and ~2M question-answer-context triplets extracted from Wikipedia.

**Disclaimer:  To facilitate the research, we release our data to the community for research-purpose use only! We do not produce any data and all the data are extracted from the Wikipedia and Web. If you find any improper content that may potentially involve copyright infringement, please contact us and we will remove the coresponding part immediately.**

Data can be downloaded [here](http://web.cse.ohio-state.edu/~yue.149/C-MORE/data/). There are two files: *C-MORE_QuestionContext.json* (~3M) and *C-MORE_QuestionAnswerContext.json* (~2M). If you only need to pretrain the retriever, you can use *C-MORE_QuestionContext.json*. If you need to pretrain both the retriever and reader for open-domain QA, you can use  *C-MORE_QuestionAnswerContext.json*. The data format of the two files are exactly the same as the format used in [DPR](https://github.com/facebookresearch/DPR). 


## Pretrained Checkpoints
We also release our pretrained [checkpoints](http://web.cse.ohio-state.edu/~yue.149/C-MORE/checkpoint/) for both retriever and reader.

## Code and Experiment
Our experiments are mostly based on the [DPR's code](https://github.com/facebookresearch/DPR). To run the experiments and replicate the results in the paper, you can follow the DPR repo to setup and add C-MORE data directory to the data config in the code. Here we give some examples:

```shell
#add the following lines to  "DPR/conf/datasets/encoder_train_default.yaml"

CMORE_train:
  _target_: dpr.data.biencoder_data.JsonQADataset
  file: "YOUR PATH TO C-MORE_QuestionContext.json"
```

Pretrain the Retriever:
```shell
python -m torch.distributed.launch --nproc_per_node=8 train_dense_encoder.py \
train=biencoder_ref \
train_datasets="[CMORE_train]" \
dev_datasets="[nq_dev]" \
output_dir=./CMORE_pretrained_retriever
```

Finetune the Retriver:
```shell
python -m torch.distributed.launch --nproc_per_node=8 train_dense_encoder.py \
train=biencoder_nq \
train_datasets="[trivia_train]" \
dev_datasets="[trivia_dev]" \
output_dir=./train_CMORE_then_trivia \
model_file=YOUR PATH TO CMORE_pretrained_retriever
```

For the reader pretraining, you need to first construct a reader training file based on the pretrained retriever and C-MORE QA pairs following the steps in [DPR's code](https://github.com/facebookresearch/DPR). And then you can run the following to pretrain the reader:

```shell
python train_extractive_reader.py \
 encoder.sequence_length=350 \
 train_files=YOUR PATH to CMORE Reader Training File \
 dev_files=YOUR PATH to CMORE Reader DEV File   \
 output_dir=./CMORE_pretrained_reader
```


