# Transforming a COCO dataset<a name="cd-coco-transform-example"></a>

Use the following Python example to transform bounding box information from a COCO format dataset into an Amazon Rekognition Custom Labels manifest file\. The code uploads the created manifest file to your Amazon S3 bucket\. The code also provides an AWS CLI command that you can use to upload your images\. 

**To transform a COCO dataset \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonS3FullAccess` permissions\. For more information, see [Step 2: Create an IAM Administrator User and Group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 2: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following Python code to transform a COCO dataset\. Set the following values\.
   + `s3_bucket` – The name of the S3 bucket in which you want to store the images and Amazon Rekognition Custom Labels manifest file\. 
   + `s3_key_path_images` – The path to where you want to place the images within the S3 bucket \(`s3_bucket`\)\.
   + `s3_key_path_manifest_file` – The path to where you want to place the Custom Labels manifest file within the S3 bucket \(`s3_bucket`\)\.
   + `local_path` – The local path to where the example opens the input COCO dataset and also saves the new Custom Labels manifest file\.
   + `local_images_path` – The local path to the images that you want to use for training\.
   + `coco_manifest` – The input COCO dataset filename\.
   + `cl_manifest_file` – A name for the manifest file created by the example\. The file is saved at the location specified by `local_path`\. By convention, the file has the extension `.manifest`, but this is not required\.
   + `job_name` – A name for the Custom Labels job\.

   ```
   import json
   import os
   import random
   import shutil
   import datetime
   import botocore
   import boto3
   import PIL.Image as Image
   import io
   
   #S3 location for images
   s3_bucket = 'bucket'
   s3_key_path_manifest_file = 'path to custom labels manifest file/'
   s3_key_path_images = 'path to images/'
   s3_path='s3://' + s3_bucket  + '/' + s3_key_path_images
   s3 = boto3.resource('s3')
   
   #Local file information
   local_path='path to input COCO dataset and output Custom Labels manifest/'
   local_images_path='path to COCO images/'
   coco_manifest = 'COCO dataset JSON file name'
   coco_json_file = local_path + coco_manifest
   job_name='Custom Labels job name'
   cl_manifest_file = 'custom_labels.manifest'
   
   label_attribute ='bounding-box'
   
   open(local_path + cl_manifest_file, 'w').close()
   
   # class representing a Custom Label JSON line for an image
   class cl_json_line:  
       def __init__(self,job, img):  
   
           #Get image info. Annotations are dealt with seperately
           sizes=[]
           image_size={}
           image_size["width"] = img["width"]
           image_size["depth"] = 3
           image_size["height"] = img["height"]
           sizes.append(image_size)
   
           bounding_box={}
           bounding_box["annotations"] = []
           bounding_box["image_size"] = sizes
   
           self.__dict__["source-ref"] = s3_path + img['file_name']
           self.__dict__[job] = bounding_box
   
           #get metadata
           metadata = {}
           metadata['job-name'] = job_name
           metadata['class-map'] = {}
           metadata['human-annotated']='yes'
           metadata['objects'] = [] 
           date_time_obj = datetime.datetime.strptime(img['date_captured'], '%Y-%m-%d %H:%M:%S')
           metadata['creation-date']= date_time_obj.strftime('%Y-%m-%dT%H:%M:%S') 
           metadata['type']='groundtruth/object-detection'
           
           self.__dict__[job + '-metadata'] = metadata
   
   
   print("Getting image, annotations, and categories from COCO file...")
   
   with open(coco_json_file) as f:
   
       #Get custom label compatible info    
       js = json.load(f)
       images = js['images']
       categories = js['categories']
       annotations = js['annotations']
   
       print('Images: ' + str(len(images)))
       print('annotations: ' + str(len(annotations)))
       print('categories: ' + str(len (categories)))
   
   
   print("Creating CL JSON lines...")
       
   images_dict = {image['id']: cl_json_line(label_attribute, image) for image in images}
   
   print('Parsing annotations...')
   for annotation in annotations:
   
       image=images_dict[annotation['image_id']]
   
       cl_annotation = {}
       cl_class_map={}
   
       # get bounding box information
       cl_bounding_box={}
       cl_bounding_box['left'] = annotation['bbox'][0]
       cl_bounding_box['top'] = annotation['bbox'][1]
    
       cl_bounding_box['width'] = annotation['bbox'][2]
       cl_bounding_box['height'] = annotation['bbox'][3]
       cl_bounding_box['class_id'] = annotation['category_id']
   
       getattr(image, label_attribute)['annotations'].append(cl_bounding_box)
   
   
       for category in categories:
            if annotation['category_id'] == category['id']:
               getattr(image, label_attribute + '-metadata')['class-map'][category['id']]=category['name']
           
       
       cl_object={}
       cl_object['confidence'] = int(1)  #not currently used by Custom Labels
       getattr(image, label_attribute + '-metadata')['objects'].append(cl_object)
   
   print('Done parsing annotations')
   
   # Create manifest file.
   print('Writing Custom Labels manifest...')
   
   for im in images_dict.values():
   
       with open(local_path+cl_manifest_file, 'a+') as outfile:
               json.dump(im.__dict__,outfile)
               outfile.write('\n')
               outfile.close()
   
   # Upload manifest file to S3 bucket.
   print ('Uploading Custom Labels manifest file to S3 bucket')
   print('Uploading'  + local_path + cl_manifest_file + ' to ' + s3_key_path_manifest_file)
   print(s3_bucket)
   s3 = boto3.resource('s3')
   s3.Bucket(s3_bucket).upload_file(local_path + cl_manifest_file, s3_key_path_manifest_file + cl_manifest_file)
   
   # Print S3 URL to manifest file,
   print ('S3 URL Path to manifest file. ')
   print('\033[1m s3://' + s3_bucket + '/' + s3_key_path_manifest_file + cl_manifest_file + '\033[0m') 
   
   # Display aws s3 sync command.
   print ('\nAWS CLI s3 sync command to upload your images to S3 bucket. ')
   print ('\033[1m aws s3 sync ' + local_images_path + ' ' + s3_path + '\033[0m')
   ```

1. Run the code\.

1. In the program output, note the `s3 sync` command\. You need it in the next step\.

1. At the command prompt, run the `s3 sync` command\. Your images are uploaded to the S3 bucket\. If the command fails during upload, run it again until your local images are synchronized with the S3 bucket\.

1. In the program output, note the S3 URL path to the manifest file\. You need it in the next step\.

1. Follow the instruction at [Creating a Manifest File](cd-manifest-files.md) to create a dataset with the uploaded manifest file\. You don't need to do steps 1\-6\. For step 14, use the Amazon S3 URL you noted in the previous step\. 

1. Build your model\. For more information, see [Training an Amazon Rekognition Custom Labels Model](tm-train-model.md)\.