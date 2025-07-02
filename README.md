# SCL
The code for paper "Semantic Contrastive Learning via VLM for Few-Shot Object Detection in Remote Sensing Image" by Bowei Yan, Gong Cheng, Chunbo Lang, and Junwei Han.

**abstract:** *Few-shot object detection (FSOD) aims to detect novel object categories with only a few annotations, but remains highly challenging due to limited supervision and high inter-class similarity in remote sensing images. One of the major obstacles is category confusion, which stems from the inability of FSOD models to learn discriminative features and construct well-separated decision boundaries, especially in visually similar categories. To address this challenge, we propose an innovative FSOD framework based on semantic contrastive learning that leverages class-level textual knowledge from a large-scale pre-trained vision-language model. Our method introduces two key components: (1) a Contrast-Aware Hyper-Weight (CAHW) module that generates adaptive classification weights by integrating semantic guidance, and (2) a Semantic Disambiguation Contrast (SDC) mechanism that aligns the visual features of proposals with textual prototypes to enhance inter-class separability. The integration of CAHW and SDC effectively mitigates category confusion between visually similar categories, enabling more robust and interpretable few-shot detection in remote sensing images. Extensive experiments on two standard benchmarks (DIOR and NWPU VHR-10.V2) demonstrate the effectiveness of our approach. The proposed method consistently outperforms existing state-of-the-art techniques across various k-shot settings and shows strong generalization capability when handling novel and semantically similar categories.*

---
![Image text](https://github.com/Ybowei/SCL/blob/main/pictures/method.jpg)
---


## 📑 Table of Contents

* Semantic Contrastive Learning via VLM for Few-Shot Object Detection in Remote Sensing Image
  * Table of Contents
  * Installation
  * Code Structure
  * Data Preparation
  * Model training and evaluation on DIOR and NWPU VHR-10.v2
    * Base training
    * Generation of contrastive knowledge dictionary
    * Model Fine-tuning
    * Evaluation
  * Model Zoo


## 🧩 Installation

Our main code is based on [MMFewShot](https://github.com/open-mmlab/mmfewshot/tree/main) and please refer to [Install.md](https://github.com/open-mmlab/mmfewshot/blob/main/docs/en/install.md) for installation of MMFewShot framwork. 
Please note that we used detectron 0.1.0 in this project. Higher versions of detectron might report errors. 
In addition, the Semantic Guidance Generator (SGG) module is built upon the following two open-source projects: [LLaVA](https://github.com/haotian-liu/LLaVA) and [RemoteCLIP](https://github.com/ChenDelong1999/RemoteCLIP).
Please refer to their respective installation instructions. Make sure both environments are correctly configured before using the SGG module.


## 🏰 Code Structure

* **configs:** Configuration files
* **checkpoints:** Checkpoint
* **Weights:** Pretraing models
* **Data:** Datasets for base training and finetuning
* **mmfewshot:** Model framework
* **Script:** Code for the SGG modules
* **Tools:** Analysis and visualize tools

## 💾 Data Preparation

* Our model is evaluated on two FSOD benchmarks DIOR and NWPU-VHR following the previous work [TFA](https://github.com/ucbdrive/few-shot-object-detection).
* Please prepare the original [DIOR](https://pan.baidu.com/s/1iLKT0JQoKXEJTGNxt5lSMg#list/path=%2F) and [NWPU VHR-10.v2](https://pan.baidu.com/s/1hqwzXeG?_at_=1728709381194#list/path=%2F) datasets and also the few-shot datasets in the folder ./data/dior and ./data/nwpu_vhr respectively.
* Please refer to [Data Preparation](https://github.com/Ybowei/UNP/blob/main/data/preparation/README.md) for more detail in the few-shot setting.

## 📖 Model training and evaluation on the both DIOR and NWPU VHR-10.v2

* We have two main steps for model training, first training the model over base classes, and then meta-fine-tuning the model over novel classes.
* The training script for base training is
  ```Python
  sh script/dist_train_base.sh

 * After model base training, we use LLaVA and RemoteCLIP models to get the contrastive knowledge dictionary, and the training script is
   ```Python
   python script/get_contrastive_text_dictionary.py
   python script/predict_dictionary_prompt_tensor.py

 * After obtaining the contrastive knowledge dictionary, we perform 3/5/10/20-shot fine-tuning over novel classes, and the training script is
   ```Python
   sh script/dist_train_finetuning.sh

 * We evaluate our model on the four splits in DIOR and two splits in VHR-10.v2. The evaluate script is
   ```Python
   sh script/dist_test.sh

 ## 📚 Model Zoo
* We provided both the base-trained models and novel-finetuning models for the both two benchmarks. The model links are [Baidu Drive]().

 ## 👏 Acknowledgement
* This repo is developed based on [TFA](https://github.com/ucbdrive/few-shot-object-detection), [LLaVA](https://github.com/haotian-liu/LLaVA), [RemoteCLIP](https://github.com/ChenDelong1999/RemoteCLIP), and [mmfewshot](https://github.com/open-mmlab/mmfewshot/tree/main). Thanks for their wonderful codebases.

