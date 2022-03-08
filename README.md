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
C-MORE contains ~3M question-answer-context triplets extracted from Wikipedia.

**Disclaimer:  To facilitate the research, we release our data to the community for research-purpose use only! We do not produce any data and all the data are extracted from the Wikipedia and Web. If you find any improper content that may potentially involve copyright infringement, please contact us and we will remove the coresponding part immediately.**

Data can be downloaded [here](). 


## Code and Experiment
Our experiments are mostly based on the [DPR's code](https://github.com/facebookresearch/DPR). To run the experiments and replicate the results in the paper, you can follow the DPR repo to setup and change the data directory to the C-MORE data.

## Pretrained Checkpoints
We also release our pretrained checkpoints for both [retriever]() and [reader]().
