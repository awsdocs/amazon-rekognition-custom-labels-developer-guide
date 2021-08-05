# Image\-Level labels in manifest files<a name="cd-manifest-files-classification"></a>

To import image\-level labels \(images labeled with scenes, concepts, or objects that don't require localization information\), you add SageMaker Ground Truth [Classification Job Output](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-data-output.html#sms-output-class) format JSON lines to a manifest file\. A manifest file is made of one or more JSON lines, one for each image that you want to import\. 

**To create a manifest file for image\-level labels**

1. Create an empty text file\.

1. Add a JSON line for each image the that you want to import\. Each JSON line should look similar to the following\.

   ```
   {"source-ref":"s3://custom-labels-console-us-east-1-nnnnnnnnnn/gt-job/manifest/IMG_1133.png","TestCLConsoleBucket":0,"TestCLConsoleBucket-metadata":{"confidence":0.95,"job-name":"labeling-job/testclconsolebucket","class-name":"Echo Dot","human-annotated":"yes","creation-date":"2020-04-15T20:17:23.433061","type":"groundtruth/image-classification"}}
   ```

1. Save the file\. You can use the extension `.manifest`, but it is not required\. 

1. Create a dataset using the manifest file that you created\. For more information, see [To create a dataset using a SageMaker Ground Truth format manifest file \(console\)](cd-manifest-files.md#create-dataset-procedure-manifest-file)\. 

 

## Image\-Level JSON Lines<a name="cd-manifest-classification-json"></a>

In this section, we show you how to create a JSON line for a single image\. Consider the following image\. A scene for the following image might be called *Sunrise*\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/sunrise.png)

The JSON line for the preceding image, with the scene *Sunrise*, might be the following\. 

```
{
    "source-ref": "s3://bucket/images/sunrise.png",
    "testdataset-classification_Sunrise": 1,
    "testdataset-classification_Sunrise-metadata": {
        "confidence": 1,
        "job-name": "labeling-job/testdataset-classification_Sunrise",
        "class-name": "Sunrise",
        "human-annotated": "yes",
        "creation-date": "2020-03-06T17:46:39.176",
        "type": "groundtruth/image-classification"
    }
}
```

Note the following information\.

### source\-ref<a name="w41aac26c17c35b9c13"></a>

\(Required\) The Amazon S3 location of the image\. The format is `"s3://BUCKET/OBJECT_PATH"`\. Images in an imported dataset must be stored in the same Amazon S3 bucket\. 

### *testdataset\-classification\_Sunrise*<a name="w41aac26c17c35b9c15"></a>

\(Required\) The label attribute\. You choose the field name\.  The field value \(1 in the preceding example\) is a label attribute identifier\. It is not used by Amazon Rekognition Custom Labels and can be any integer value\. There must be corresponding metadata identified by the field name with *\-metadata* appended\. For example, `"testdataset-classification_Sunrise-metadata"`\. 

### *testdataset\-classification\_Sunrise*\-metadata<a name="w41aac26c17c35b9c17"></a>

\(Required\) Metadata about the label attribute\. The field name must be the same as the label attribute with *\-metadata* appended\. 

*confidence*  
\(Required\) Currently not used by Amazon Rekognition Custom Labels but a value between 0 and 1 must be supplied\. 

*job\-name*  
\(Optional\) A name that you choose for the job that processes the image\. 

*class\-name*  
\(Required\) A class name that you choose for the scene or concept that applies to the image\. For example, `"Sunrise"`\. 

*human\-annotated*  
\(Required\) Specify `"yes"`, if the annotation was completed by a human\. Otherwise `"no"`\. 

*creation\-date*   
\(Required\) The Coordinated Universal Time \(UTC\) date and time that the label was created\. 

*type*  
\(Required\) The type of processing that should be applied to the image\. For image\-level labels, the value is `"groundtruth/image-classification"`\. 

### Adding multiple image\-level labels to an image<a name="cd-classification-multiple-labels"></a>

You can add multiple labels to an image\. For example, the follow JSON adds two labels, *football* and *ball* to a single image\. 

```
{
    "source-ref": "S3 bucket location", 
    "sport0":0, # FIRST label
    "sport0-metadata": { 
        "class-name": "football", 
        "confidence": 0.8, 
        "type":"groundtruth/image-classification", 
        "job-name": "identify-sport", 
        "human-annotated": "yes", 
        "creation-date": "2018-10-18T22:18:13.527256" 
    },
    "sport1":1, # SECOND label
    "sport1-metadata": { 
        "class-name": "ball", 
        "confidence": 0.8, 
        "type":"groundtruth/image-classification", 
        "job-name": "identify-sport", 
        "human-annotated": "yes", 
        "creation-date": "2018-10-18T22:18:13.527256" 
    }
}  # end of annotations for 1 image
```