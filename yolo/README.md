# YOLO Object Detectors, You Only Look Once

[![Paper](http://img.shields.io/badge/Paper-arXiv.1804.02767-B3181B?logo=arXiv)](https://arxiv.org/abs/1804.02767)
[![Paper](http://img.shields.io/badge/Paper-arXiv.2004.10934-B3181B?logo=arXiv)](https://arxiv.org/abs/2004.10934)

This repository is the unofficial implementation of the following paper. However, we spent painstaking hours ensuring that every aspect that we constructed was the exact same as the original paper and the original repository.

* YOLOv3: An Incremental Improvement: [YOLOv3: An Incremental Improvement](https://arxiv.org/abs/1804.02767)

* YOLOv4: Optimal Speed and Accuracy of Object Detection: [YOLOv4: Optimal Speed and Accuracy of Object Detection](https://arxiv.org/abs/2004.10934)

## Description

> :memo: Provide description of the model.  
>  
> * Provide brief information of the algorithms used.  
> * Provide links for demos, blog posts, etc.  

Yolo v1 the original implementation was released in 2015 providing a ground breaking algorithm that would quickly process images, and locate objects in a single pass through the detector. The original implementation based used a backbone derived from state of the art object classifier of the time, like [GoogLeNet](https://arxiv.org/abs/1409.4842) and [VGG](https://arxiv.org/abs/1409.1556). More attention was given to the novel Yolo Detection head that allowed for Object Detection with a single pass of an image. Though limited, the network could predict up to 90 bounding boxes per image, and was tested for about 80 classes per box. Also, the model could only make prediction at one scale. These attributes caused yolo v1 to be more limited, and less versatile, so as the year passed, the Developers continued to update and develop this model.

Yolo v3 and v4 serve as the most up to date and capable versions of the Yolo network group. These model uses a custom backbone called Darknet53 that uses knowledge gained from the ResNet paper to improve its predictions. The new backbone also allows for objects to be detected at multiple scales. As for the new detection head, the model now predicts the bounding boxes using a set of anchor box priors (Anchor Boxes) as suggestions. The multiscale predictions in combination with the Anchor boxes allows for the network to make up to 1000 object predictions on a single image. Finally, the new loss function forces the network to make better prediction by using Intersection Over Union (IOU) to inform the models confidence rather than relying on the mean squared error for the entire output.

## Authors

* Vishnu Samardh Banna ([@GitHub vishnubanna](https://github.com/vishnubanna))
* Anirudh Vegesana ([@GitHub anivegesana](https://github.com/anivegesana))
* Akhil Chinnakotla ([@GitHub The-Indian-Chinna](https://github.com/The-Indian-Chinna))
* Tristan Yan ([@GitHub Tyan3001](https://github.com/Tyan3001))
* Naveen Vivek ([@GitHub naveen-vivek](https://github.com/naveen-vivek))

## Table of Contents

* [Our Goal](#our-goal)
* [Models in the library](#models-in-the-library)
* [Requirements](#requirements)
* [Results](#results)
* [Dataset](#dataset)
* [Build Instructions](#build-instructions)
* [Example Usage](#example-usage)
* [Training](#training)
* [Evaluation](#evaluation)
* [References](#references)
* [License](#license)
* [Citation](#citation)

## Our Goal
Our goal with this model conversion is to provide highly versatile implementations of the Backbone and Yolo Head. We have tried to build the model in such a way that the Yolo head could easily be connected to a new, more powerful backbone if a person chose to.

## Models in the library

| Object Detectors | Classifiers      |
| :--------------: | :--------------: |
| Yolo-v3          | Darknet53        |
| Yolo-v3 tiny     | CSPDarknet53     |
| Yolo-v3 spp      |
| Yolo-v4          |
| Yolo-v4 tiny     |

For all Standard implementations, we provided scripts to load the weights into the Tensorflow implementation directly from the original Darknet Implementation, provided that you have a yolo**.cfg file, and the corresponding yolo**.weights file.

## Data Pipeline/Dataset Benchmarking
The Data Pipeline is found within the dataloaders folder. The way to use our data pipeline is by typing "from yolo.dataloaders import preprocessing_functions.py as pf" at the top of your python file. Then loading in the tfds dataset by means of tfds.ImageFolder or tfds.load. Then use the following function as seen below:

    dataset = pf.preprocessing(dataset, data_augmentation_split, preprocessing_type, size, batch_size, num_of_classes, shuffle_flag)

    ARGS:
        dataset (tfds.data.Dataset): The Dataset you would like to preprocess.
        data_augmentation_split (int): The percentage of the dataset that is data
            augmented.
        preprocessing_type (str): The type of preprocessing should be conducted
            and is dependent on the type of training.
        size (int): The size of the dataset being passed into preprocessing.
        batch_size (int): The size of the each batch.
        num_of_classes (int): The number of classes found within the dataset.
        shuffle_flag (bool): This is a Flag that determines whether to or not to shuffle
            within the function.

## Results

[![TensorFlow Hub](https://img.shields.io/badge/TF%20Hub-Models-FF6F00?logo=tensorflow)](https://tfhub.dev/...)

> :memo: Provide a table with results. (e.g., accuracy, latency)  
>  
> * Provide links to the pre-trained models (checkpoint, SavedModel files).  
>   * Publish TensorFlow SavedModel files on TensorFlow Hub (tfhub.dev) if possible.  
> * Add links to [TensorBoard.dev](https://tensorboard.dev/) for visualizing metrics.  
>  
> An example table for image classification results  
### object
| Model Name | Width | latency  |FPS (GPU)  | mAP   |   
|:------------:|:------------:|:----------:|:-------------------:|:----------------:|  
| Yolov3 | 416 |35ms     | 40        | 59.7% |  
| Yolov3-spp | 608 |40ms     | 30        | 61.7% |
| Yolov3-tiny | 416 |20ms     | 60        | not tested yet |    


> ### Image Classification  
>  
> | Model name | Download | Top 1 Accuracy | Top 5 Accuracy |  
> |------------|----------|----------------|----------------|  
> | Model name | [Checkpoint](https://drive.google.com/...), [SavedModel](https://tfhub.dev/...) | xx% | xx% |  

## Requirements

[![TensorFlow 2.4](https://img.shields.io/badge/TensorFlow-2.2-FF6F00?logo=tensorflow)](https://github.com/tensorflow/tensorflow/releases/tag/v2.2.0)
[![Python 3.8](https://img.shields.io/badge/Python-3.8-3776AB)](https://www.python.org/downloads/release/python-380/)

> :memo: Provide details of the software required.  
>  
> * Add a `requirements.txt` file to the root directory for installing the necessary dependencies.  
>   * Describe how to install requirements using pip.  
> * Alternatively, create INSTALL.md.  

To install requirements:

```setup
pip install -r requirements.txt
```

## Build Instructions

> :memo: Provide Building an using the model

```
from yolo.utils.run_utils import prep_gpu
from yolo.configs import yolo as exp_cfg
from yolo.tasks.yolo import YoloTask
prep_gpu()

config = exp_cfg.YoloTask()  
task = YoloTask(config)
model = task.build_model()
task.initialize(model)
model.summary()
```

## Example Usage

> :memo: Examples for all supported models

## Training

> :memo: Provide training information.  
>  
> * Provide details for preprocessing, hyperparameters, random seeds, and environment.  
> * Provide a command line example for training.  

Please run this command line for training.

```python

```
```shell
python3 ...
```

## Evaluation

> :memo: Provide an evaluation script with details of how to reproduce results.  
>  
> * Describe data preprocessing / postprocessing steps.  
> * Provide a command line example for evaluation.  

Please run this command line for evaluation.

```shell
python3 ...
```

## Change Log

> :memo: Provide a changelog.

## References

> :memo: Provide links to references.  

## License

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

> :memo: Place your license text in a file named LICENSE in the root of the repository.  
>  
> * Include information about your license.  
> * Reference: [Adding a license to a repository](https://help.github.com/en/github/building-a-strong-community/adding-a-license-to-a-repository)  

This project is licensed under the terms of the **Apache License 2.0**.

## Citation

> :memo: Make your repository citable.  
>  
> * Reference: [Making Your Code Citable](https://guides.github.com/activities/citable-code/)  

If you want to cite this repository in your research paper, please use the following information.
