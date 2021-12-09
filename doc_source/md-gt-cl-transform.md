# Transforming multi\-label SageMaker Ground Truth manifest files<a name="md-gt-cl-transform"></a>

This topic shows you how to transform a multi\-label Amazon SageMaker Ground Truth manifest file to an Amazon Rekognition Custom Labels format manifest file\. 

SageMaker Ground Truth manifest files for multi\-label jobs are formatted differently than Amazon Rekognition Custom Labels format manifest files\. Multi\-label classification is when an image is classified into a set of classes, but might belong to multiple classes at once\. In this case, the image can potentially have multiple labels \(multi\-label\), such as *football* and *ball*\.

For information about multi\-label SageMaker Ground truth jobs, see [Image Classification \(Multi\-label\)](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-image-classification-multilabel.html)\. For information about multi\-label format Amazon Rekognition Custom Labels manifest files, see [Adding multiple image\-level labels to an image](md-create-manifest-file-classification.md#md-dataset-purpose-classification-multiple-labels)\.

## Getting the manifest file for a SageMaker Ground Truth job<a name="md-get-gt-manifest"></a>

The following procedure shows you how to get the output manifest file \(`output.manifest`\) for a SageMaker Ground Truth job\. You use `output.manifest` as input to the next procedure\.

**To download a SageMaker Ground Truth job manifest file**

1. Open the [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\. 

1. In the navigation pane, choose **Ground Truth** and then choose **Labeling Jobs**\. 

1. Choose the labeling job that contains the manifest file that you want to use\.

1. On the details page, choose the link under **Output dataset location**\. The Amazon S3 console is opened at the dataset location\. 

1. Choose `Manifests`, `output` and then `output.manifest`\.

1. Choose **Object Actions** and then choose **Download** to download the manifest file\.

## Transforming a multi\-label SageMaker manifest file<a name="md-transform-ml-gt"></a>

The following procedure creates a multi\-label format Amazon Rekognition Custom Labels manifest file from an existing multi\-label format SageMaker GroundTruth manifest file\.

**Note**  
To run the code, you need Python version 3, or higher\.<a name="md-procedure-multi-label-transform"></a>

**To transform a multi\-label SageMaker manifest file**

1. Use the following example code\. Change the following parameters:
   + `job_name` to a name for the job\.
   + `ground_truth_manifest_file` to path and file name of the SageMaker Ground Truth manifest file that you want to use\.
   + `new_manifest_file` to a name for the new Amazon Rekognition Custom Labels format manifest file\.

   ```
   #!/usr/bin/env python
   # coding: utf-8
   
   import json
   
   
   #you can set job_name to anything containing alphanumical characters and '-'s
   job_name = 'image-label-attribute-name'
   
   #ground_truth_manifest_file should be set to the name of your existing manifest file, including the relative path.
   ground_truth_manifest_file = 'examplefromdocumentation.manifest'
   
   #new_manifest_file is a new file that will be created containing the proper format accepted by Amazon Rekognition Custom Labels
   new_manifest_file = 'new-examplefromdocumentation.manifest'
   
   
   #first we want to read the existing lines from the sagemaker groundtruth manifest file into memory
   with open(ground_truth_manifest_file) as f:
       lines = f.readlines()
   
   
   #now we will iterate through the lines one at a time to generate the new lines for the updated manifest file
   with open(new_manifest_file, 'w') as the_new_file:
       for line in lines:
           
           #first load in the old json item from the groundtruth manifest file
           old_json = json.loads(line)
           new_json = {}
           
           #set the location of the image
           new_json['source-ref'] = old_json['source-ref']
           
           #temporarily store the list of labels
           labels = old_json[job_name]
           
           #then we will iterate through the labels one by one and reformat them to the Custom Labels format
           for index, label in enumerate(labels):
               new_json[f'{job_name}{index}'] = index
               metadata = {}
               metadata['class-name'] = old_json[f'{job_name}-metadata']['class-map'][str(label)]
               metadata['confidence'] = old_json[f'{job_name}-metadata']['confidence-map'][str(label)]
               metadata['type'] = old_json[f'{job_name}-metadata']['type']
               metadata['job-name'] = old_json[f'{job_name}-metadata']['job-name']
               metadata['human-annotated'] = old_json[f'{job_name}-metadata']['human-annotated']
               metadata['creation-date'] = old_json[f'{job_name}-metadata']['creation-date']
               
               #add the metadata to new json line
               new_json[f'{job_name}{index}-metadata'] = metadata
               
           #write the current line to the json file
           the_new_file.write(json.dumps(new_json))
           the_new_file.write('\n')
   ```

1. Run the python code\. The manifest file is created and named with the value you supplied for `new_manifest_file`\.

1. Do [Creating a manifest file](md-create-manifest-file.md) to create an Amazon Rekognition Custom Labels dataset with the new manifest file\.
**Note**  
You don't need to do step 6 as the images are already uploaded to an Amazon S3 bucket\. Make sure Amazon Rekognition Custom Labels has access to the Amazon S3 bucket referenced in the `soure-ref` field of the manifest file JSON lines\. For more information, see [Accessing external Amazon S3 Buckets](su-console-policy.md#su-external-buckets)\. If your Ground Truth job stores images in the Amazon Rekognition Custom Labels Console Bucket, you don't need to add permissions\.