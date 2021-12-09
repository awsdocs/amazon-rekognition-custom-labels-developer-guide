# Fixing training errors<a name="tm-debugging-fixing-validation-errors"></a>

You use the manifest summary to identify [Terminal manifest content errors](tm-debugging.md#tm-error-category-combined-terminal) and [Non terminal JSON line validation errors](tm-debugging.md#tm-error-category-non-terminal-errors) encountered during training\. You must fix manifest content errors\. We recommend that you also fix non\-terminal JSON Line errors\. For information about specific errors, see [Non\-Terminal JSON Line Validation Errors](tm-debugging-json-line-errors.md) and [Terminal manifest content errors](tm-debugging-aggregate-errors.md)\.

You can makes fixes to the training or testing dataset used for training\. Alternatively, you can make the fixes in the training and testing validation manifest files and use them to train the model\. 

After you make your fixes, you need to import the updated manifests\(s\) and retrain the model\. For more information, see [Creating a manifest file](md-create-manifest-file.md)\.

The following procedure shows you how to use the manifest summary to fix terminal manifest content errors\. The procedure also shows you how to locate and fix JSON Line errors in the training and testing validation manifests\. 

**To fix Amazon Rekognition Custom Labels training errors**

1. Download the validation results files\. The file names are *training\_manifest\_with\_validation\.json*, *testing\_manifest\_with\_validation\.json* and *manifest\_summary\.json*\. For more information, see [Getting the validation results](tm-debugging-getting-validation-data.md)\. 

1. Open the manifest summary file \(*manifest\_summary\.json*\)\. 

1. Fix any errors in the manifest summary\. For more information, see [Understanding the manifest summary](tm-debugging-summary.md)\.

1. In the manifest summary, iterate through the `error_line_indices` array in `training` and fix the errors in `training_manifest_with_validation.json` at the corresponding JSON Line numbers\. For more information, see [Understanding training and testing validation result manifests](tm-debugging-scope-json-line.md)\.

1. Iterate through the `error_line_indices` array in `testing` and fix the errors in `testing_manifest_with_validation.json` at the corresponding JSON Line numbers\.

1. Retrain the model using the validation manifest files as the training and testing datasets\. For more information, see [Training an Amazon Rekognition Custom Labels model](training-model.md)\. 

If you are using the AWS SDK and choose to fix the errors in the training or the test validation data manifest files, use the location of the validation data manifest files in the [TrainingData](https://docs.aws.amazon.com/rekognition/latest/dg/API_TrainingData) and [TestingData](https://docs.aws.amazon.com/rekognition/latest/dg/API_TestingData) input parameters to [CreateProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_CreateProjectVersion)\. For more information, see [Training a model \(SDK\)](training-model.md#tm-sdk)\. 

## JSON line error precedence<a name="tm-debugging-json-line-error-precedence"></a>

The following JSON Line errors are detected first\. If any of these errors occur, validation of JSON Line errors is stopped\. You must fix these errors before you can fix any of the other JSON Line errors 
+ MISSING\_SOURCE\_REF
+ ERROR\_INVALID\_SOURCE\_REF\_FORMAT
+ ERROR\_NO\_LABEL\_ATTRIBUTES
+ ERROR\_INVALID\_LABEL\_ATTRIBUTE\_FORMAT
+ ERROR\_INVALID\_LABEL\_ATTRIBUTE\_METADATA\_FORMAT
+ ERROR\_MISSING\_BOUNDING\_BOX\_CONFIDENCE
+ ERROR\_MISSING\_CLASS\_MAP\_ID
+ ERROR\_INVALID\_JSON\_LINE