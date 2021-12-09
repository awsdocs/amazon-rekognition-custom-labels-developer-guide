# Accessing Amazon Rekognition Custom Labels training results \(SDK\)<a name="im-metrics-api"></a>

The Amazon Rekognition API provides metrics beyond those provided in the console\. 

Like the console, the API provides access to the following metrics as summary information for the training results and as training results for each label:
+ [Precision](im-metrics-use.md#im-precision-metric)
+ [Recall](im-metrics-use.md#im-recall-metric)
+ [F1](im-metrics-use.md#im-f1-metric)

The average threshold for all labels and the threshold for individual labels is returned\.

The API also provides the following metrics for classification and image detection \(object location on image\)\.
+ *Confusion Matrix* for image classification\.
+ *Mean Average Precision \(mAP\)* for image detection\.
+ *Mean Average Recall \(mAR\)* for image detection\.

The API also provides true positive, false positive, false negative, and true negative values\. For more information, see [Metrics for evaluating your model](im-metrics-use.md)\.

The aggregate F1 score metric is returned directly by the API\. Other metrics are accessible from a [Summary file](im-summary-file-api.md) and [Evaluation manifest snapshot](im-evaluation-manifest-snapshot-api.md) files stored in an Amazon S3 bucket\. For more information, see [Accessing the summary file and evaluation manifest snapshot \(SDK\)](im-access-summary-evaluation-manifest.md)\.

**Topics**
+ [Summary file](im-summary-file-api.md)
+ [Evaluation manifest snapshot](im-evaluation-manifest-snapshot-api.md)
+ [Accessing the summary file and evaluation manifest snapshot \(SDK\)](im-access-summary-evaluation-manifest.md)
+ [Reference: Training results summary file](im-summary-file.md)