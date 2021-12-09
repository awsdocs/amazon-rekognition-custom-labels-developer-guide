# Tagging a model<a name="tm-tagging-model"></a>

You can identify, organize, search for, and filter your Amazon Rekognition models by using tags\. Each tag is a label consisting of a user\-defined key and value\. For example, to help determine billing for your models, you could tag your models with a `Cost center` key and add the appropriate cost center number as a value\. For more information, see [Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html)\.

Use tags to:
+ Track billing for a model by using cost allocation tags\. For more information, see [Using Cost Allocation Tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html)\.
+ Control access to a model by using Identity and Access Management \(IAM\)\. For more information, see [Controlling access to AWS resources using resource tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_tags.html)\.
+ Automate model management\. For example, you can run automated start or stop scripts that turn off development models during non\-business hours to reduce costs\. For more information, see [Running a trained Amazon Rekognition Custom Labels model](running-model.md)\. 

You can tag models by using the Amazon Rekognition console or by using the AWS SDKs\. 

**Topics**
+ [Tagging models \(console\)](#tm-tagging-model-console)
+ [Viewing model tags](#tm-tagging-model-viewing-console)
+ [Tagging models \(SDK\)](#tm-tagging-model-sdk)

## Tagging models \(console\)<a name="tm-tagging-model-console"></a>

You can use the Rekognition console to add tags to models, view the tags attached to a model, and remove tags\. 

### Adding or removing tags<a name="tm-tagging-model-add-remove-console"></a>

This procedure explains how to add tags to, or remove tags from, an existing model\. You can also add tags to a new model when it is trained\. For more information, see [Training an Amazon Rekognition Custom Labels model](training-model.md)\. 

**To add tags to, or remove tags from, an existing model using the console**

1. Open the Amazon Rekognition console at [ https://console\.aws\.amazon\.com/rekognition/]( https://console.aws.amazon.com/rekognition/)\.

1. Choose **Get started**\. 

1. In the navigation pane, choose **Projects**\.

1. On the **Projects** resources page, choose the project that contains the model that you want to tag\.

1. In the navigation pane, under the project you previously chose, choose **Models**\.

1. In the **Models** section, choose the model that you want to add a tag to\. 

1. On the model's details page, choose the **Tags** tab\. 

1. In the **Tags** section, choose **Manage tags**\.

1. On the **Manage tags** page, choose **Add new tag**\.

1. Enter a key and a value\.

   1. For **Key**, enter a name for the key\.

   1. For **Value**, enter a value\.

1. To add more tags, repeat steps 9 and 10\.

1. \(Optional\) To remove a tag, choose **Remove** next to the tag that you want to remove\. If you are removing a previously saved tag, it is removed when you save your changes\. 

1. Choose **Save changes** to save your changes\.

## Viewing model tags<a name="tm-tagging-model-viewing-console"></a>

You can use the Amazon Rekognition console to view the tags attached to a model\.

To view the tags attached to *all models within a project*, you must use the AWS SDK\. For more information, see [Listing model tags](#listing-model-tags-sdk)\.

**To view the tags attached to a model**

1. Open the Amazon Rekognition console at [ https://console\.aws\.amazon\.com/rekognition/]( https://console.aws.amazon.com/rekognition/)\.

1. Choose **Get started**\. 

1. In the navigation pane, choose **Projects**\.

1. On the **Projects** resources page, choose the project that contains the model whose tag you want to view\.

1. In the navigation pane, under the project you previously chose, choose **Models**\.

1. In the **Models** section, choose the model whose tag you want to view\. 

1. On the model's details page, choose the **Tags** tab\. The tags are shown in **Tags** section\.

## Tagging models \(SDK\)<a name="tm-tagging-model-sdk"></a>

You can use the AWS SDK to:
+ Add tags to a new model
+ Add tags to an existing model
+ List the tags attached to a model 
+ Remove tags from a model 

The tags in the following AWS CLI examples are in the following format\.

```
--tags '{"key1":"value1","key2":"value2"}' 
```

Alternatively, you can use this format\.

```
--tags key1=value1,key2=value2
```

If you haven't installed the AWS CLI, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

### Adding tags to a new model<a name="tagging-new-model-sdk"></a>

You can add tags to a model when you create it using the [CreateProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_CreateProjectVersion) operation\. Specify one or more tags in the `Tags` array input parameter\. 

```
aws rekognition create-project-version --project-name "project name"\
  --output-config '{ "S3Location": { "Bucket": "output bucket", "Prefix":  "output folder" } }'\
  --tags '{"key1":"value1","key2":"value2"}'
```

For information about creating and training a model, see [Training a model \(SDK\)](training-model.md#tm-sdk)\.

### Adding tags to an existing model<a name="tagging-new-model-sdk"></a>

To add one or more tags to an existing model, use the [TagResource](https://docs.aws.amazon.com/rekognition/latest/dg/API_TagResource) operation\. Specify the model's Amazon Resource Name \(ARN\) \(`ResourceArn`\) and the tags \(`Tags`\) that you want to add\. The following example shows how to add two tags\.

```
aws rekognition tag-resource --resource-arn resource-arn \
--tags '{"key1":"value1","key2":"value2"}'
```

You can get the ARN for a model by calling [CreateProjectVersion](https://docs.aws.amazon.com/rekognition/latest/dg/API_CreateProjectVersion)\.

### Listing model tags<a name="listing-model-tags-sdk"></a>

To list the tags attached to a model, use the [ListTagsForResource](https://docs.aws.amazon.com/rekognition/latest/dg/API_ListTagsForResource) operation and specify the ARN of the model \(`ResourceArn`\)\. The response is a map of tag keys and values that are attached to the specified model\.

```
aws rekognition list-tags-for-resource --resource-arn resource-arn
```

The output displays a list of the tags attached to the model\.

```
{
    "Tags": {
        "Dept": "Engineering",
        "Name": "Ana Silva Carolina",
        "Role": "Developer"
    }
}
```

To see which models in a project have a specific tag, call `DescribeProjectVersions` to get a list of models\. Then call `ListTagsForResource` for each model in the response from `DescribeProjectVersions`\. Inspect the response from `ListTagsForResource` to see if the required tag is present\. 

The following Python 3 example shows you how search all your projects for a specific tag key and value\. The output includes the project ARN and the model ARN where a matching key is found\.

**To search for a tag value**

1. Save the following code to a file named `find_tag.py`\.

   ```
   # Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
   # SPDX-License-Identifier: Apache-2.0
   """
   Purpose
   Shows how to find a tag value that's associated with models within
   your Amazon Rekognition Custom Labels projects.
   To run: python find_tag.py tag tag_value
   """
   import logging
   import argparse
   import boto3
   
   from botocore.exceptions import ClientError
   
   
   logger = logging.getLogger(__name__)
   
   
   def find_tag_in_projects(rekognition_client, key, value):
       """
       Finds Amazon Rekognition Custom Label models tagged with the supplied key and key value.
       :param rekognition_client: An Amazon Rekognition boto3 client.
       :param key: The tag key to find.
       :param value: The value of the tag that you want to find.
       return: A list of matching model versions (and model projects) that were found.
       """
       try:
   
           found_tags = []
           found = False
   
           projects = rekognition_client.describe_projects()
           # Iterate through each project and models within a project.
           for project in projects["ProjectDescriptions"]:
               logger.info("Searching project: %s ...", project["ProjectArn"])
   
               models = rekognition_client.describe_project_versions(
                   ProjectArn=(project["ProjectArn"])
               )
   
               for model in models["ProjectVersionDescriptions"]:
                   logger.info("Searching model %s", model["ProjectVersionArn"])
   
                   tags = rekognition_client.list_tags_for_resource(
                       ResourceArn=model["ProjectVersionArn"]
                   )
   
                   logger.info(
                       "\tSearching model: %s for tag: %s value: %s.",
                       model["ProjectVersionArn"],
                       key,
                       value,
                   )
                   # Check if tag exists
   
                   if key in tags["Tags"]:
                       if tags["Tags"][key] == value:
                           found = True
                           logger.info(
                               "\t\tMATCH: Project: %s: model version %s",
                               project["ProjectArn"],
                               model["ProjectVersionArn"],
                           )
                           found_tags.append(
                               {
                                   "Project": project["ProjectArn"],
                                   "ModelVersion": model["ProjectVersionArn"],
                               }
                           )
   
           if found is False:
               logger.info("No match for Tag %s with value %s.", key, value)
           return found_tags
       except ClientError as err:
           logger.info("Problem finding tags: %s. ", format(err))
           raise
   
   
   def main():
       """
       Entry point for example.
       """
       logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
       # Set up command line arguments.
       parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
   
       parser.add_argument("tag", help="The tag that you want to find.")
       parser.add_argument("value", help="The tag value that you want to find.")
   
       args = parser.parse_args()
       key = args.tag
       value = args.value
   
       print(
           "Searching your models for tag: {tag} with value: {value}.".format(
               tag=key, value=value
           )
       )
   
       rekognition_client = boto3.client("rekognition")
   
       # Get tagged models for all projects.
       tagged_models = find_tag_in_projects(rekognition_client, key, value)
   
       print("Matched models\n--------------")
       if len(tagged_models) > 0:
           for model in tagged_models:
               print(
                   "Project: {project}\nModel version: {version}\n".format(
                       project=model["Project"], version=model["ModelVersion"]
                   )
               )
   
       else:
           print("No matches found.")
   
       print("Done.")
   
   
   if __name__ == "__main__":
       main()
   ```

1. At the command prompt enter the following\. Replace *key* and *value* with the key name and the key value that you want to find\.

   ```
   python find_tag.py key value
   ```

### Deleting tags from a model<a name="tm-removing-a-tag-sdk"></a>

To remove one or more tags from a model, use the [UntagResource](https://docs.aws.amazon.com/rekognition/latest/dg/API_UntagResource) operation\. Specify the ARN of the model \(`ResourceArn`\) and the tag keys \(`Tag-Keys`\) that you want to remove\. 

```
aws rekognition untag-resource --resource-arn resource-arn \
  --tag-keys '["key1","key2"]'
```

Alternatively, you can specify `tag-keys` in this format

```
--tag-keys key1,key2 
```