# Terminal manifest file errors<a name="tm-terminal-errors-reference"></a>

This topic describes the [Terminal manifest file errors](tm-debugging.md#tm-error-category-terminal)\. Manifest file errors do not have an associated error code\. The validation results manifests are not created when a terminal manifest file error occurs\. For more information, see [Understanding the manifest summary](tm-debugging-summary.md)\. Terminal manifest errors prevent the reporting of [Non\-Terminal JSON Line Validation Errors](tm-debugging-json-line-errors.md)\. 

## The manifest file extension or contents are invalid\.<a name="tm-error-message-ERROR_MANIFEST_INACCESSIBLE_OR_UNSUPPORTED_FORMAT"></a>

The training or testing manifest file doesn't have a file extension or its contents are invalid\. 

**To fix error *The manifest file extension or contents are invalid\.***
+ Check the following possible causes in both the training and testing manifest files\.
  + The manifest file is missing a file extension\. By convention the file extension is `.manifest`\.
  +  The Amazon S3 bucket or key for the manifest file couldn't be found\.

## The manifest file is empty\.<a name="tm-error-message-ERROR_EMPTY_MANIFEST"></a>



The training or testing manifest file used for training exists, but it is empty\. The manifest file needs a JSON Line for each image that you use for training and testing\.

**To fix error *The manifest file is empty\.***

1. Check which of the training or testing manifests are empty\.

1. Add JSON Lines to the empty manifest file\. For more information, see [Creating a manifest file](md-create-manifest-file.md)\. Alternatively, create a new dataset with the console\. For more information, see [Creating training and test datasets \(Console\)](md-create-dataset.md)\.



## The manifest file size exceeds the maximum supported size\.<a name="tm-error-message-ERROR_MANIFEST_SIZE_TOO_LARGE"></a>



The training or testing manifest file size \(in bytes\) is too large\. For more information, see [Guidelines and quotas in Amazon Rekognition Custom Labels](limits.md)\. A manifest file can have less than the maximum number of JSON Lines and still exceed the maximum file size\.

You can't use the Amazon Rekognition Custom Labels console to fix error *The manifest file size exceeds the maximum supported size*\.

**To fix error *The manifest file size exceeds the maximum supported size\.***

1. Check which of the training and testing manifests exceed the maximum file size\.

1. Reduce the number of JSON Lines in the manifest files that are too large\. For more information, see [Creating a manifest file](md-create-manifest-file.md)\.

## The S3 bucket permissions are incorrect\.<a name="tm-error-message-ERROR_INVALID_PERMISSIONS_MANIFEST_S3_BUCKET"></a>

Amazon Rekognition Custom Labels doesn't have permissions to one or more of the buckets containing the training and testing manifest files\. 

You can't use the Amazon Rekognition Custom Labels console to fix this error\.

**To fix error *The S3 bucket permissions are incorrect\.***
+ Check the permissions for the bucket\(s\) containing the training and testing manifests\. For more information, see [Step 4: Set up Amazon Rekognition Custom Labels permissions](su-console-policy.md)\.

## Unable to write to output S3 bucket\.<a name="tm-error-message-ERROR_CANNOT_WRITE_OUTPUT_S3_BUCKET"></a>



The service is unable to generate the training output files\.

**To fix error *Unable to write to output S3 bucket\.***
+ Check that the Amazon S3 bucket information in the [OutputConfig](https://docs.aws.amazon.com/rekognition/latest/dg/API_OutputConfig) input parameter to [CreateProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_CreateProjectVersion) is correct\. 

You can't use the Amazon Rekognition Custom Labels console to fix this error\.