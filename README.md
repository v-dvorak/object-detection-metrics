# Object Detection Metrics

This project is as fork [Rafael Padilla](https://github.com/rafaelpadilla/)'s Object-detection-metrics. It removes the UI elements.

## Table of contents

- [Supported bounding box formats](#supported-bounding-box-formats)
- [Metrics](#metrics)
    - [AP with IOU Threshold *t=0.5*](#ap-with-iou-threshold-t05)
    - [mAP with IOU Threshold *t=0.5*](#map-with-iou-threshold-t05)
    - [AP@.5 and AP@.75](#ap5-and-ap75)
    - [AP@[.5:.05:.95]](#ap50595)

## Supported bounding box formats

This implementation does not require modifications of the detection models to match complicated input formats, avoiding
conversions to XML, JSON, CSV, or other file types. It supports more than 8 different kinds of annotation formats,
including the most popular ones, as presented in the Table below.

|                                  Annotation tool                                  |      Annotation types       |                                               Output formats                                               |
|:---------------------------------------------------------------------------------:|:---------------------------:|:----------------------------------------------------------------------------------------------------------:|
|                  [Label me](https://github.com/wkentaro/labelme)                  | Bounding boxes and polygons |                       Labelme format, but provides conversion to COCO and PASCAL VOC                       |
|                 [LabelIMG](https://github.com/tzutalin/labelImg)                  |       Bounding boxes        |                                            PASCAL VOC and YOLO                                             |
|                [Microsoft VoTT](https://github.com/Microsoft/VoTT)                | Bounding boxes and polygons | PASCAL VOC, TFRecords, specific CSV, Azure Custom Vision Service, Microsoft Cognitive Toolkit (CNTK), VoTT |
| [Computer Vision Annotation Tool (CVAT)](https://github.com/openvinotoolkit/cvat) | Bounding boxes and polygons |                            COCO, CVAT, Labelme, PASCAL VOC, TFRecord, YOLO, etc                            |
| [VGG Image Annotation Tool (VIA)](https://www.robots.ox.ac.uk/~vgg/software/via/) | Bounding boxes and polygons |                                       COCO and specific CSV and JSON                                       |

## Metrics

As each dataset adopts a specific annotation format, works tend to use the evaluation tools provided by the datasets
considered to test the performance of their methods, what makes their results dependent to the implemented metric type.
PASCAL VOC dataset uses the PASCAL VOC annotation format, which provides a MATLAB evaluation code of the metrics AP and
mAP (IOU=.50) hampering other types of metrics to be reported with this dataset. The following table shows that among
the listed methods, results are reported using a total of 14 different metrics. Due to the fact that the evaluation
metrics are directly associated with the annotation format of the datasets, almost all works report their results using
only the metrics implemented by the benchmarking datasets, making such cross-datasets comparative results quite rare in
the object detection literature.

|    Method    |             Benchmark dataset              |                                     Metrics                                     |
|:------------:|:------------------------------------------:|:-------------------------------------------------------------------------------:|
|  CornerNet   |                    COCO                    |                 AP@[.5:.05:.95]; AP@.50; AP@.75; APS; APM; APL                  |
| EfficientDet |                    COCO                    |                         AP@[.5:.05:.95]; AP@.50; AP@.75                         |
|  Fast R-CNN  |        PASCAL VOC 2007, 2010, 2012         |                                AP; mAP (IOU=.50)                                |
| Faster R-CNN |           PASCAL VOC 2007, 2012            |                                AP; mAP (IOU=.50)                                |
| Faster R-CNN |                    COCO                    |                             AP@[.5:.05:.95]; AP@.50                             |
|    R-CNN     |        PASCAL VOC 2007, 2010, 2012         |                                AP; mAP (IOU=.50)                                |
|   RFB Net    |                  VOC 2007                  |                                  mAP (IOU=.50)                                  |
|   RFB Net    |                    COCO                    |                 AP@[.5:.05:.95]; AP@.50; AP@.75; APS; APM; APL                  |
|  RefineDet   |               VOC 2007, 2012               |                                  mAP (IOU=.50)                                  |
|  RefineDet   |                    COCO                    |                 AP@[.5:.05:.95]; AP@.50; AP@.75; APS; APM; APL                  |
|  RetinaNet   |                    COCO                    |                 AP@[.5:.05:.95]; AP@.50; AP@.75; APS; APM; APL                  |
|    R-FCN     |               VOC 2007, 2012               |                                  mAP (IOU=.50)                                  |
|    R-FCN     |                    COCO                    |                      AP@[.5:.05:.95];AP@.50; APS; APM; APL                      |
|     SSD      |               VOC 2007, 2012               |                                  mAP (IOU=.50)                                  |
|     SSD      |                    COCO                    | AP@[.5:.05:.95]; AP@.50; AP@.75; APS; APM; APL; AR1; AR10; AR100; ARS; ARM; ARL |
|     SSD      |                  ImageNet                  |                                  mAP (IOU=.50)                                  |
|   Yolo v1    | PASCAL VOC 2007, 2012; Picasso; People-Art |                                AP; mAP (IOU=.50)                                |
|   Yolo v2    |           PASCAL VOC 2007, 2012            |                                AP; mAP (IOU=.50)                                |
|   Yolo v2    |                    COCO                    | AP@[.5:.05:.95]; AP@.50; AP@.75; APS; APM; APL; AR1; AR10; AR100; ARS; ARM; ARL |
|   Yolo v3    |                    COCO                    | AP@[.5:.05:.95]; AP@.50; AP@.75; APS; APM; APL; AR1; AR10; AR100; ARS; ARM; ARL |
|   Yolo v4    |                    COCO                    |                 AP@[.5:.05:.95]; AP@.50; AP@.75; APS; APM; APL                  |
|   Yolo v5    |                    COCO                    |                             AP@[.5:.05:.95]; AP@.50                             |

As previously presented, there are different ways to evaluate the area under the precision x recall and recall x IOU
curves. Nonetheless, besides the combinations of different IOU thresholds and interpolation points, other considerations
are also applied resulting in different metric values. Some methods limit the evaluation by object scales and detections
per image. Such variations are computed and named differently as shown below:

### AP with IOU Threshold *t=0.5*

This AP metric is widely used to evaluate detections in the PASCAL VOC dataset. It measures the AP of each class
individually by computing the area under the precision x recall curve interpolating all points. In order to classify
detections as TP or FP the IOU threshold is set to *t=0.5*.

### mAP with IOU Threshold *t=0.5*

This metric is also used by PASCAL VOC dataset and is calculated as the AP with IOU *t=0.5*, but the result obtained by
each class is averaged.

### AP@.5 and AP@.75

These two metrics evaluate the precision x curve differently than the PASCAL VOC metrics. In this method, the
interpolation is performed in *N=101* recall points. Then, the computed results for each class are summed up and divided
by the number of classes.

The only difference between AP@.5 and AP@.75 is the applied IOU thresholds. AP@.5 uses *t=0.5* whereas AP@.75 applies
*t=0.75*. These metrics are commonly used to report detectAP<sub>S</sub>, AP<sub>M</sub> and AP<sub>L</sub>ions
performed in the COCO dataset.

### AP@[.5:.05:.95]

This metric expands the AP@.5 and AP@.75 metrics by computing the AP@ with ten different IOU thresholds (
*t=[0.5, 0.55, ..., 0.95]*) and taking the average among all computed results.

## Requirements

```
Python 3.11
auto_mix_prep==0.2.0
matplotlib==3.9.2
numpy==2.1.2
pandas==2.2.3
setuptools==65.5.0
```

## Acknowledgments

Based on `rafaelpadilla`'s [work](https://github.com/rafaelpadilla/review_object_detection_metrics)
that was published in
the [Journal Electronics - Special Issue Deep Learning Based Object Detection](https://www.mdpi.com/2079-9292/10/3/279).

```
@Article{electronics10030279,
AUTHOR = {Padilla, Rafael and Passos, Wesley L. and Dias, Thadeu L. B. and Netto, Sergio L. and da Silva, Eduardo A. B.},
TITLE = {A Comparative Analysis of Object Detection Metrics with a Companion Open-Source Toolkit},
JOURNAL = {Electronics},
VOLUME = {10},
YEAR = {2021},
NUMBER = {3},
ARTICLE-NUMBER = {279},
URL = {https://www.mdpi.com/2079-9292/10/3/279},
ISSN = {2079-9292},
DOI = {10.3390/electronics10030279}
}
```