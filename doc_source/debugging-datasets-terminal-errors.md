# Terminal errors<a name="debugging-datasets-terminal-errors"></a>

 There are two types of terminal errors — file errors that cause dataset creation to fail, and content errors that Amazon Rekognition Custom Labels removes from the dataset\. Dataset creation fails if there are too many content errors\.

**Topics**
+ [Terminal file errors](#debugging-datasets-terminal-file-errors)
+ [Terminal content errors](#debugging-datasets-terminal-content-errors)

## Terminal file errors<a name="debugging-datasets-terminal-file-errors"></a>

The following are file errors\. You can get information about file errors by calling `DescribeDataset` and checking the `Status` and `StatusMessage` fields\. For example code, see [Describing a dataset \(SDK\)](md-describing-dataset-sdk.md)\.
+ [ERROR\_MANIFEST\_INACCESSIBLE\_OR\_UNSUPPORTED\_FORMAT](#md-error-status-ERROR_MANIFEST_INACCESSIBLE_OR_UNSUPPORTED_FORMAT)
+ [ERROR\_MANIFEST\_SIZE\_TOO\_LARGE](#md-error-status-ERROR_MANIFEST_SIZE_TOO_LARGE)\.
+ [ERROR\_MANIFEST\_ROWS\_EXCEEDS\_MAXIMUM](#md-error-status-ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM)
+ [ERROR\_INVALID\_PERMISSIONS\_MANIFEST\_S3\_BUCKET](#md-error-status-ERROR_INVALID_PERMISSIONS_MANIFEST_S3_BUCKET)
+ [ERROR\_TOO\_MANY\_RECORDS\_IN\_ERROR](#md-error-status-ERROR_TOO_MANY_RECORDS_IN_ERROR)
+ [ERROR\_MANIFEST\_TOO\_MANY\_LABELS](#md-error-status-ERROR_MANIFEST_TOO_MANY_LABELS)
+ [ERROR\_INSUFFICIENT\_IMAGES\_PER\_LABEL\_FOR\_DISTRIBUTE](#md-error-status-ERROR_INSUFFICIENT_IMAGES_PER_LABEL_FOR_DISTRIBUTE)

### ERROR\_MANIFEST\_INACCESSIBLE\_OR\_UNSUPPORTED\_FORMAT<a name="md-error-status-ERROR_MANIFEST_INACCESSIBLE_OR_UNSUPPORTED_FORMAT"></a>

#### Error message<a name="md-error-message-ERROR_MANIFEST_INACCESSIBLE_OR_UNSUPPORTED_FORMAT"></a>

The manifest file extension or contents are invalid\.

The training or testing manifest file doesn't have a file extension or its contents are invalid\. 

**To fix error *ERROR\_MANIFEST\_INACCESSIBLE\_OR\_UNSUPPORTED\_FORMAT***
+ Check the following possible causes in both the training and testing manifest files\.
  + The manifest file is missing a file extension\. By convention the file extension is `.manifest`\.
  +  The Amazon S3 bucket or key for the manifest file couldn't be found\.

### ERROR\_MANIFEST\_SIZE\_TOO\_LARGE<a name="md-error-status-ERROR_MANIFEST_SIZE_TOO_LARGE"></a>

#### Error message<a name="md-error-message-ERROR_MANIFEST_SIZE_TOO_LARGE"></a>

The manifest file size exceeds the maximum supported size\.

The training or testing manifest file size \(in bytes\) is too large\. For more information, see [Guidelines and quotas in Amazon Rekognition Custom Labels](limits.md)\. A manifest file can have less than the maximum number of JSON Lines and still exceed the maximum file size\.

You can't use the Amazon Rekognition Custom Labels console to fix error *The manifest file size exceeds the maximum supported size*\.

**To fix error *ERROR\_MANIFEST\_SIZE\_TOO\_LARGE***

1. Check which of the training and testing manifests exceed the maximum file size\.

1. Reduce the number of JSON Lines in the manifest files that are too large\. For more information, see [Creating a manifest file](md-create-manifest-file.md)\.

### ERROR\_MANIFEST\_ROWS\_EXCEEDS\_MAXIMUM<a name="md-error-status-ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM"></a>

#### Error message<a name="md-error-message-ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM"></a>

The manifest file has too many rows\.

#### More information<a name="md-error-description-ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM"></a>

The number of JSON Lines \(number of images\) in the manifest file is greater than the allowed limit\. The limit is different for image\-level models and object location models\. For more information, see [Guidelines and quotas in Amazon Rekognition Custom Labels](limits.md)\. 

JSON Line error are validated until the number of JSON Lines reaches the `ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM` limit\. 

You can't use the Amazon Rekognition Custom Labels console to fix error `ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM`\.

**To fix `ERROR_MANIFEST_ROWS_EXCEEDS_MAXIMUM`**
+ Reduce the number of JSON Lines in the manifest\. For more information, see [Creating a manifest file](md-create-manifest-file.md)\.



### ERROR\_INVALID\_PERMISSIONS\_MANIFEST\_S3\_BUCKET<a name="md-error-status-ERROR_INVALID_PERMISSIONS_MANIFEST_S3_BUCKET"></a>

#### Error message<a name="md-error-message-ERROR_INVALID_PERMISSIONS_MANIFEST_S3_BUCKET"></a>

The S3 bucket permissions are incorrect\.

Amazon Rekognition Custom Labels doesn't have permissions to one or more of the buckets containing the training and testing manifest files\. 

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

**To fix error *ERROR\_INVALID\_PERMISSIONS\_MANIFEST\_S3\_BUCKET***
+ Check the permissions for the bucket\(s\) containing the training and testing manifests\. For more information, see [Step 4: Set up Amazon Rekognition Custom Labels permissions](su-console-policy.md)\.

### ERROR\_TOO\_MANY\_RECORDS\_IN\_ERROR<a name="md-error-status-ERROR_TOO_MANY_RECORDS_IN_ERROR"></a>

#### Error message<a name="md-error-message-ERROR_TOO_MANY_RECORDS_IN_ERROR"></a>

 The manifest file has too many terminal errors\.

**To fix `ERROR_TOO_MANY_RECORDS_IN_ERROR`**
+ Reduce the number of JSON Lines \(images\) with terminal content errors\. For more information, see [Terminal manifest content errors](tm-debugging-aggregate-errors.md)\. 

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

### ERROR\_MANIFEST\_TOO\_MANY\_LABELS<a name="md-error-status-ERROR_MANIFEST_TOO_MANY_LABELS"></a>

#### Error message<a name="md-error-message-ERROR_MANIFEST_TOO_MANY_LABELS"></a>

The manifest file has too many labels\.

##### More information<a name="md-error-description-ERROR_MANIFEST_TOO_MANY_LABELS"></a>

The number of unique labels in the manifest \(dataset\) is more than the allowed limit\. If the training dataset is split to create a testing dataset, the mumber of labels is determined after the split\. 

**To fix ERROR\_MANIFEST\_TOO\_MANY\_LABELS \(Console\)**
+ Remove labels from the dataset\. For more information, see [Managing labels](md-labels.md)\. The labels are automatically removed from the images and bounding boxes in your dataset\.



**To fix ERROR\_MANIFEST\_TOO\_MANY\_LABELS \(JSON Line\)**
+ Manifests with image level JSON Lines – If the image has a single label, remove the JSON Lines for images that use the desired label\. If the JSON Line contains multiple labels, remove only the JSON object for the desired label\. For more information, see [Adding multiple image\-level labels to an image](md-create-manifest-file-classification.md#md-dataset-purpose-classification-multiple-labels)\. 

  Manifests with object location JSON Lines – Remove the bounding box and associated label information for the label that you want to remove\. Do this for each JSON Line that contains the desired label\. You need to remove the label from the `class-map` array and corresponding objects in the `objects` and `annotations` array\. For more information, see [Object localization in manifest files](md-create-manifest-file-object-detection.md)\.

### ERROR\_INSUFFICIENT\_IMAGES\_PER\_LABEL\_FOR\_DISTRIBUTE<a name="md-error-status-ERROR_INSUFFICIENT_IMAGES_PER_LABEL_FOR_DISTRIBUTE"></a>

#### Error message<a name="md-error-message-ERROR_MANIFEST_TOO_MANY_LABELS"></a>

The manifest file doesn't have enough labeled images to distribute the dataset\.



Dataset distribution occurs when Amazon Rekognition Custom Labels splits a training dataset to create a test dataset\. You can also split a dataset by calling the `DistributeDatasetEntries` API\.

**To fix error *ERROR\_MANIFEST\_TOO\_MANY\_LABELS***
+ Add more labeled images to the training dataset

## Terminal content errors<a name="debugging-datasets-terminal-content-errors"></a>

The following are terminal content errors\. During dataset creation, images that have terminal content errors are removed from the dataset\. The dataset can still be used for training\. If there are too many content errors, dataset/update fails\. Terminal content errors related to dataset operations aren't displayed in the console or returned from `DescribeDataset` or other API\. If you notice that images or annotations are missing from your datasets, check your dataset manifest files for the following issues: 
+ The length of a JSON line is too long\. The maximum length is 100,000 characters\.
+ The `source-ref` value is missing from a JSON Line\.
+ The format of a `source-ref` value in a JSON Line is invalid\.
+ The contents of a JSON Line are not valid\.
+ The value a `source-ref` field appears more than once\. An image can only be referenced once in a dataset\.

For information about the `source-ref` field, see [Creating a manifest file](md-create-manifest-file.md)\. 