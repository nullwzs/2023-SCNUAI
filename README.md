# 2023-SCNUAI-contest
第四届华南师大人工智能算法挑战赛-第八名解决方案
=====

队伍名称：SCNU_米奇妙妙屋

最终得分：89.25334

## 使用注意事项
1. 本仓库目录下 ./data/wenshu_ms_dataset/dev/ 文件夹下的数据用于展示，并不完整。完整数据参见下方链接。

2. 基于Bert的TextCNN模型和StructBert模型的训练集由于文件太大，在本仓库中并未上传，有需要者可自行运行data_process文件夹中脚本文件获取，或从以下网盘链接的方式获取：

- **TextCNN 训练集数据** -> 链接：https://pan.baidu.com/s/1-tWrbI4kquUhWIKM70wmVQ?pwd=scnu 提取码：scnu 

- **StructBert 训练集数据** -> 链接：https://pan.baidu.com/s/1jyQH5HMf08e4R-aLzklbgg?pwd=scnu 提取码：scnu 

- **训练模型pth文件** -> 链接：https://pan.baidu.com/s/13lRI44VRRT475pCABZuZVQ?pwd=scnu 提取码：scnu 

<br>

## 赛题说明
### 比赛内容

&emsp;&emsp;题目：**法律类案检索**

&emsp;&emsp;比赛网址：https://aistudio.baidu.com/aistudio/competition/detail/899/0/introduction 

&emsp;&emsp;类案检索作为人工智能支持司法审判的重要内容，对于提升法院整体裁判水平、实现类案适法统一、促进司法公正有极其重要的积极意义。


### 比赛任务

&emsp;&emsp;给定若干查询案例，每一个查询案例均对应一个大小为100的候选案例池给定一个查询文本，要求从候选案例池中筛选出与查询案例相关的类案，即对100个候选案件进行相似度排序，使得类似案件尽可能排在前面。

&emsp;&emsp;输入：查询文本、候选案件

&emsp;&emsp;输出：候选案例相似度由高到低的排名列表

### 数据集获取
&emsp;&emsp;类案检索任务数据集使用中国民事类案检索数据集，包含15181个查询案例和总数为1518100的候选案例池，所有案例文本均来自公开的中国民事案件判决书，涉及84种类别的民事案件。数据集由若干<查询文本，候选集，类案序号>组成。比赛数据集划分为训练集、测试集和验证集，训练集共8500条数据，测试集共2181条数据，验证集共4500条数据，其中验证集的数据不包含gt_idx字段。

<br>

**完整数据获取请参见下方链接：**

**https://aistudio.baidu.com/aistudio/datasetdetail/205651**

<br>

## 实现平台
1. 对于TextCNN模型，本团队在本地构建模型代码，并上传至**Kaggle平台**，进行GPU加速训练，一共训练了30个epoch，34类标签在测试集上的平均准确率为 0.86
2. 对于StructBert模型，本团队在阿里云的**ModelScope平台**实现，共训练了7个epoch，二分类标签在测试集上的准确率为 0.8386

## 实验结果
|Model|Attribute|is_split|Learning_rate|Epoch|Filter_nums|Batch_size|Optimizer|Train:Test:Dev|Accurancy(Test/NDCG@30)|Note|
|:---:|:-------:|:------:|:-----------:|:---:|:---------:|:--------:|:-------:|:------------:|:---------------------:|:---|
|bm25(Baseline)|None|None|None|None|None|None|None|None|None/59.93|None|
|TextCNN|cat1|no|1e-3|6|256|50|Adam|6:2:2|0.91/58.70538|10cls|
|TextCNN|cat1+cat2|yes|1e-3|6|256|50|Adam|6:2:2|(0.91/0.86)/82.58402|10cls+6cls|
|TextCNN|cat1+cat2|yes|5e-5~1e-3|3|512|50|Adam|6:2:2|(0.92/0.90)/83.87502|10cls+6cls(subdivision:g2&g6)|
|TextCNN+StructBert|cat1+cat2+JudgeAccusation|yes/yes|1e-3|3/20/10|256|50|Adam|6:2:2|(0.91/0.86/0.8205)/85.09863|10cls+6cls+2cls|
|TextCNN+StructBert|cat1+cat2+JudgeAccusation|yes/yes|5e-5~1e-3|3/20/10|256|50|Adam|6:2:2|(0.91/0.90/0.8205)/87.90274|10cls+6cls(subdivision:g2&g6)|
|TextCNN+StructBert|category+JudgeAccusation|no/yes|5e-5~1e-3|6/10|256|50|Adam|6:2:2|(0.8537/0.8205)/87.91883|34cls+2cls|
|TextCNN+StructBert|category+JudgeAccusation|no/yes|2e-6~5e-5|6/10|512|16|Adam|6:2:2|(0.8612/0.8205)/88.58282|34cls+2cls|
|TextCNN+StructBert|category+JudgeAccusation|no/no|2e-6~5e-5|12/7|512|16|Adam|6:2:2|(0.8674/0.8386)/89.07108|34cls+2cls|
|TextCNN+StructBert|category+JudgeAccusation|no/no|2e-6~5e-5|30/7|512|16|Adam|6:2:2|(0.8693/0.8386)/89.25334|34cls+2cls|



## 参考文献
```
@article{wang2019structbert,
  title={Structbert: Incorporating language structures into pre-training for deep language understanding},
  author={Wang, Wei and Bi, Bin and Yan, Ming and Wu, Chen and Bao, Zuyi and Xia, Jiangnan and Peng, Liwei and Si, Luo},
  journal={arXiv preprint arXiv:1908.04577},
  year={2019}
}
```

## 文件目录

```
main
├─main.ipynb
├─README.md
├─data
│  └─wenshu_ms_dataset
│      └─dev
├─data_process
│  ├─data_process for Bert_classifier0.ipynb
│  ├─data_process for Bert_classifier1.ipynb
│  ├─data_process for Bert_classifier2.ipynb
│  ├─data_process for textcnn.ipynb
│  ├─data_process for textcnn2.ipynb
│  └─data_process for textmatch.ipynb
├─result
│  ├─dev_ans_v5-5.json
│  ├─structbert_pred_res-v3.csv
│  └─textcnn_pred_res-v5.xlsx
├─StructBert-classifier
│  ├─classifier.ipynb
│  └─workspace
│      ├─output
│      └─tmp
└─TextCNN-classifier
    ├─TextCNN-test&predict.ipynb
    ├─TextCNN-train.ipynb
    └─models
```

