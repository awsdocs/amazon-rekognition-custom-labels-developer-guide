# Accessing the summary file and evaluation manifest snapshot \(SDK\)<a name="im-access-summary-evaluation-manifest"></a>

To get training results, you call [DescribeProjectVersions](https://docs.aws.amazon.com/rekognition/latest/dg/API_DescribeProjectVersions)\. For example code, see [Describing a model \(SDK\)](md-describing-model-sdk.md)\.

The location of the metrics is returned in the `ProjectVersionDescription` response from `DescribeProjectVersions`\.
+ `EvaluationResult` – The location of the summary file\.
+ `TestingDataResult` – The location of the evaluation manifest snapshot used for testing\. 

The F1 score and summary file location are returned in `EvaluationResult`\. For example:

```
"EvaluationResult": {
                "F1Score": 1.0,
                "Summary": {
                    "S3Object": {
                        "Bucket": "echo-dot-scans",
                        "Name": "test-output/EvaluationResultSummary-my-echo-dots-project-v2.json"
                    }
                }
            }
```

The evaluation manifest snapshot is stored in the location specified in the ` --output-config` input parameter that you specified in [Training a model \(SDK\)](training-model.md#tm-sdk)\. 

**Note**  
The amount of time, in seconds, that you are billed for training is returned in `BillableTrainingTimeInSeconds`\. 

For information about the metrics that are returned by the Amazon Rekognition Custom Labels, see [Accessing Amazon Rekognition Custom Labels training results \(SDK\)](im-metrics-api.md)\.