# Analyzing an image with a trained model<a name="detecting-custom-labels"></a>

To analyze an image with a trained Amazon Rekognition Custom Labels model, you call the [DetectCustomLabels](https://docs.aws.amazon.com/rekognition/latest/dg/API_DetectCustomLabels) API\. The result from `DetectCustomLabels` is a prediction that the image contains specific objects, scenes, or concepts\.

To call `DetectCustomLabels`, you specify the following:
+ The Amazon Resource Name \(ARN\) of the Amazon Rekognition Custom Labels model that you want to use\.
+ The image that you want the model to make a prediction with\. You can provide an input image as an image byte array \(base64\-encoded image bytes\), or as an Amazon S3 object\. For more information, see [Image](https://docs.aws.amazon.com/rekognition/latest/dg/API_Image)\.



Custom labels are returned in an array of [Custom Label](https://docs.aws.amazon.com/rekognition/latest/dg/API_CustomLabel) objects\. Each custom label represents a single object, scene, or concept found in the image\. A custom label includes:
+ A label for the object, scene, or concept found in the image\.
+ A bounding box for objects found in the image\. The bounding box coordinates show where the object is located on the source image\. The coordinate values are a ratio of the overall image size\. For more information, see [BoundingBox](https://docs.aws.amazon.com/rekognition/latest/dg/API_BoundingBox)\. `DetectCustomLabels` returns bounding boxes only if the model is trained to detect object locations\.
+ The confidence that Amazon Rekognition Custom Labels has in the accuracy of the label and bounding box\.

  To filter labels based on the detection confidence, specify a value for `MinConfidence` that matches your desired confidence level\. For example, if you need to be very confident of the prediction, specify a high value for `MinConfidence`\. To get all labels, regardless of confidence, specify a `MinConfidence` value of 0\. 

The performance of your model is measured, in part, by the recall and precision metrics calculated during model training\. For more information, see [Metrics for evaluating your model](im-metrics-use.md)\.

To increase the precision of your model, set a higher value for `MinConfidence`\. For more information, see [Reducing false positives \(better precision\)](tr-improve-model.md#im-reduce-false-positives)\.

To increase the recall of your model, use a lower value for `MinConfidence`\. For more information, see [Reducing false negatives \(better recall\)](tr-improve-model.md#im-reduce-false-negatives)\. 

If you don't specify a value for `MinConfidence`, Amazon Rekognition Custom Labels returns a label based on the assumed threshold for that label\. For more information, see [Assumed threshold](im-metrics-use.md#im-assumed-threshold)\. You can get the value of the assumed threshold for a label from the model's training results\. For more information, see [Training a model \(Console\)](training-model.md#tm-console)\.

By using the `MinConfidence` input parameter, you are specifying a desired threshold for the call\. Labels detected with a confidence below the value of `MinConfidence` aren't returned in the response\. Also, the assumed threshold for a label doesn't affect the inclusion of the label in the response\.

**Note**  
Amazon Rekognition Custom Labels metrics express an assumed threshold as a floating point value between 0\-1\. The range of `MinConfidence` normalizes the threshold to a percentage value \(0\-100\)\. Confidence responses from DetectCustomLabels are also returned as a percentage\. 

 You might want to specify a threshold for specific labels\. For example, when the precision metric is acceptable for Label A, but not for Label B\. When specifying a different threshold \(`MinConfidence`\), consider the following\. 
+ If you're only interested in a single label \(A\), set the value of `MinConfidence` to the desired threshold value\. In the response, predictions for label A are returned \(along with other labels\) only if the confidence is greater than `MinConfidence`\. You need to filter out any other labels that are returned\. 
+ If you want to apply different thresholds to multiple labels, do the following:

  1. Use a value of 0 for `MinConfidence`\. A value 0 ensures that all labels are returned, regardless of the detection confidence\.

  1. For each label returned, apply the desired threshold by checking that the label confidence is greater than the threshold that you want for the label\.

For more information, see [Improving a trained Amazon Rekognition Custom Labels model](improving-model.md)\.

If you're finding the confidence values returned by `DetectCustomLabels` are too low, consider retraining the model\. For more information, see [Training an Amazon Rekognition Custom Labels model](training-model.md)\. You can restrict the number of custom labels returned from `DetectCustomLabels` by specifying the `MaxResults` input parameter\. The results are returned sorted from the highest confidence to the lowest\. 

For other examples that call `DetectCustomLabels`, see [Examples](examples.md)\.

For information about securing `DetectCustomLabels`, see [Securing DetectCustomLabels](sc-introduction.md#sc-detect-custom-labels)\.

**To detect custom labels \(API\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` and `AmazonS3ReadOnlyAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Train and deploy your model\. For more information, see [](getting-started.md)\.

1. Ensure the IAM user calling `DetectCustomLabels` has access to the model you used in step 3\. For more information, see [Securing DetectCustomLabels](sc-introduction.md#sc-detect-custom-labels)\.

1. Upload an image that contains text to an S3 bucket\. 

   For instructions, see [Uploading Objects into Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/UploadingObjectsintoAmazonS3.html) in the *Amazon Simple Storage Service User Guide*\. The Python and Java 2 examples also show you how to use a local image file to pass an image by using raw bytes\. The file must be smaller than 4 MB\.

1. Use the following examples to call the `DetectCustomLabels` operation\. The Python and Java examples show the image and overlay the analysis results, similar to the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/rekognition/latest/customlabels-dg/images/prediction.jpg)

------
#### [ AWS CLI ]

   This AWS CLI command displays the JSON output for the `DetectCustomLabels` CLI operation\. Change the values of the following input parameters\. 
   + `bucket` with the name of Amazon S3 bucket that you used in step 4\. 
   + `image` with the name of the input image file you uploaded in step 4\.
   + `projectVersionArn` with the ARN of the model that you want to use\. 

   ```
   aws rekognition detect-custom-labels --project-version-arn "model_arn"\
      --image '{"S3Object":{"Bucket":"bucket","Name":"image"}}'\
      --min-confidence 70
   ```

------
#### [ Python ]

   The following example code displays bounding boxes around custom labels detected in an image\. Replace the following values: 
   + `bucket` with the name of Amazon S3 bucket that you used in step 4\. 
   + `image` with the name of the input image file you uploaded in step 4\.
   + `min_confidence` with the minimum confidence \(0\-100\)\. Objects detected with a confidence lower than this value are not returned\.
   + `model` with the ARN of the model that you want use with the input image\. 

**Note**  
To use a local image, uncomment the following lines in the fuction `main`\. Remember to set the value of `photo` to the path and filename of your local image\.  

   ```
   label_count=analyze_local_image(rek_client,
               model,
               photo,
               min_confidence)
   ```

   ```
   #Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   #PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   import boto3
   import io
   import logging
   from PIL import Image, ImageDraw, ImageFont
   
   from botocore.exceptions import ClientError
   
   logger = logging.getLogger(__name__)
   
   def analyze_local_image(rek_client, model, photo, min_confidence):
       """
       Analyzes an image stored as a local file.
       :param rek_client: The Amazon Rekognition Boto3 client.
       :param s3_connection: The Amazon S3 Boto3 S3 connection object.
       :param model: The ARN of the Amazon Rekognition Custom Labels model that you want to use.
       :param photo: The name and file path of the photo that you want to analyze.
       :param min_confidence: The desired threshold/confidence for the call.
       """
   
       try:
           logger.info ("Analyzing local file: %s", photo)
           image=Image.open(photo) 
           image_type=Image.MIME[image.format]
   
           if (image_type == "image/jpeg" or image_type== "image/png") == False:
               logger.error("Invalid image type for %s", photo)
               raise ValueError(
                   f"Invalid file format. Supply a jpeg or png format file: {photo}"
               )
               
           # get images bytes for call to detect_anomalies
           image_bytes = io.BytesIO()
           image.save(image_bytes, format=image.format)
           image_bytes = image_bytes.getvalue()
   
           response = rek_client.detect_custom_labels(Image={'Bytes': image_bytes},
               MinConfidence=min_confidence,
               ProjectVersionArn=model)
   
           show_image (image, response)
           return len(response['CustomLabels'])
   
       except ClientError as err:
           logger.error(format(err))
           raise
       
   
   def analyze_s3_image(rek_client,s3_connection, model,bucket,photo, min_confidence):
       """
       Analyzes an image stored in the specified S3 bucket.
       :param rek_client: The Amazon Rekognition Boto3 client.
       :param s3_connection: The Amazon S3 Boto3 S3 connection object.
       :param model: The ARN of the Amazon Rekognition Custom Labels model that you want to use.
       :param bucket: The name of the S3 bucket that contains the image that you want to analyze.
       :param photo: The name of the photo that you want to analyze.
       :param min_confidence: The desired threshold/confidence for the call.
       """
   
       try:
           #Get image from S3 bucket.
           
           logger.info("analyzing bucket: %s image: %s", bucket, photo)
           s3_object = s3_connection.Object(bucket,photo)
           s3_response = s3_object.get()
           
   
           stream = io.BytesIO(s3_response['Body'].read())
           image=Image.open(stream)
   
           image_type=Image.MIME[image.format]
   
           if (image_type == "image/jpeg" or image_type== "image/png") == False:
               logger.error("Invalid image type for %s", photo)
               raise ValueError(
                   f"Invalid file format. Supply a jpeg or png format file: {photo}")
                   
   
           img_width, img_height = image.size  
           draw = ImageDraw.Draw(image)  
           
           #Call DetectCustomLabels 
           response = rek_client.detect_custom_labels(Image={'S3Object': {'Bucket': bucket, 'Name': photo}},
               MinConfidence=min_confidence,
               ProjectVersionArn=model)
   
           show_image (image, response)
           return len(response['CustomLabels'])
   
       except ClientError as err:
           logger.error(format(err))
           raise
   
   def show_image(image, response):
       """
       Displays the analyzed image and overlays analysis results
       :param image: The analyzed image
       :param response: the response from DetectCustomLabels
       """
       try: 
           font_size=30
           line_width=5
   
           img_width, img_height = image.size  
           draw = ImageDraw.Draw(image)  
                   
           # calculate and display bounding boxes for each detected custom label       
           image_level_label_height = 0
           
           for custom_label in response['CustomLabels']:
               confidence=int(round(custom_label['Confidence'],0))
               label_text=f"{custom_label['Name']}:{confidence}%"
               fnt = ImageFont.truetype('/Library/Fonts/Tahoma.ttf', font_size)
               text_width, text_height = draw.textsize(label_text,fnt)
   
               logger.info(f"Label: {custom_label['Name']}") 
               logger.info(f"Confidence:  {confidence}%")
   
               # Draw bounding boxes, if present
               if 'Geometry' in custom_label:
                   box = custom_label['Geometry']['BoundingBox']
                   left = img_width * box['Left']
                   top = img_height * box['Top']
                   width = img_width * box['Width']
                   height = img_height * box['Height']
                   
                   logger.info("Bounding box")
                   logger.info("\tLeft: {0:.0f}".format(left))
                   logger.info("\tTop: {0:.0f}".format(top))
                   logger.info("\tLabel Width: {0:.0f}".format(width))
                   logger.info("\tLabel Height: {0:.0f}".format(height))
                   
                   points = (
                       (left,top),
                       (left + width, top),
                       (left + width, top + height),
                       (left , top + height),
                       (left, top))
                   #Draw bounding box and label text
                   draw.line(points, fill="limegreen", width=line_width)
                   draw.rectangle([(left + line_width , top+line_width), (left + text_width + line_width, top + line_width + text_height)],fill="black")
                   draw.text((left + line_width ,top +line_width), label_text, fill="limegreen", font=fnt) 
               
               #draw image-level label text.
               else:
                   draw.rectangle([(10 , image_level_label_height), (text_width + 10, image_level_label_height+text_height)],fill="black")
                   draw.text((10,image_level_label_height), label_text, fill="limegreen", font=fnt)  
   
                   image_level_label_height += text_height
               
           image.show()
   
       except Exception as err:
           logger.error(format(err))
           raise
           
   
   def main():
   
       try:
           logging.basicConfig(level=logging.INFO, format="%(levelname)s: %(message)s")
   
           bucket="bucket"
           photo="image"
           model="model_arn"
           min_confidence=60
   
           rek_client=boto3.client('rekognition')
           
           s3_connection = boto3.resource('s3')
           label_count=analyze_s3_image(rek_client,
               s3_connection,
               model,
               bucket,
               photo,
               min_confidence)
   
           """
           # Uncomment to analyze a local file. 
           # Change photo to the path and file name of a local file.
           label_count=analyze_local_image(rek_client,
               model,
               photo,
               min_confidence)
           
           """ 
           logger.info(f"Custom labels detected: {label_count}")
   
       except ClientError as err:
           print("A service client error occurred: " + format(err.response["Error"]["Message"]))
       except ValueError as err:
           print ("A value error occurred: " + format(err))
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java ]

   The following example code displays bounding boxes around custom labels detected in an image\. In the function `main`, replace the following values: 
   + `bucket` with the name of Amazon S3 bucket that you used in step 4\. 
   + `photo` with the name of the input image file you uploaded in step 4\.
   + `minConfidence` with the minimum confidence \(0\-100\)\. Objects detected with a confidence lower than this value are not returned\.
   + `projectVersionArn` with the ARN of the model that you want use with the input image\. 

   ```
   //Copyright 2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   package com.example.rekognition;
   
   //Import the basic graphics classes.
   import java.awt.*;
   import java.awt.image.BufferedImage;
   import java.util.List;
   import javax.imageio.ImageIO;
   import javax.swing.*;
   
   import com.amazonaws.services.rekognition.AmazonRekognition;
   import com.amazonaws.services.rekognition.AmazonRekognitionClientBuilder;
   
   import com.amazonaws.services.rekognition.model.BoundingBox;
   import com.amazonaws.services.rekognition.model.CustomLabel;
   import com.amazonaws.services.rekognition.model.DetectCustomLabelsRequest;
   import com.amazonaws.services.rekognition.model.DetectCustomLabelsResult;
   import com.amazonaws.services.rekognition.model.Image;
   import com.amazonaws.services.rekognition.model.S3Object;
   import com.amazonaws.services.s3.AmazonS3;
   import com.amazonaws.services.s3.AmazonS3ClientBuilder;
   import com.amazonaws.services.s3.model.S3ObjectInputStream;
   
   // Calls DetectCustomLabels and displays a bounding box around each detected image.
   public class DetectCustomLabels extends JPanel {
   
   
       private static final long serialVersionUID = 1L;
   
       BufferedImage image;
       static int scale;
       DetectCustomLabelsResult result;
   
       public  DetectCustomLabels(DetectCustomLabelsResult labelsResult, BufferedImage bufImage) throws Exception {
           super();
           scale = 4; // increase to shrink image size.
   
           result = labelsResult;
           image = bufImage;
   
           
       }
       // Draws the bounding box around the detected custom labels.
       public void paintComponent(Graphics g) {
           float left = 0;
           float top = 0;
           int height = image.getHeight(this);
           int width = image.getWidth(this);
   
           Graphics2D g2d = (Graphics2D) g; // Create a Java2D version of g.
   
           // Draw the image.
           g2d.drawImage(image, 0, 0, width / scale, height / scale, this);
           g2d.setColor(new Color(0, 212, 0));
   
           // Iterate through results and display bounding boxes.
           List<CustomLabel> customLabels = result.getCustomLabels();
           for (CustomLabel customLabel : customLabels) {
               
           	if (customLabel.getGeometry()!=null) {
           		BoundingBox box = customLabel.getGeometry().getBoundingBox();
           		left = width * box.getLeft();
           		top = height * box.getTop();
           		g2d.drawString(customLabel.getName(), left/scale, top/scale);
           		g2d.drawRect(Math.round(left / scale), Math.round(top / scale),
                       Math.round((width * box.getWidth()) / scale), Math.round((height * box.getHeight())) / scale);
           	}
               
           }
       }
   
   
       public static void main(String arg[]) throws Exception {
           		
           String photo = "photo";
           String bucket = "bucket";
           String projectVersionArn ="model_arn";		
           float minConfidence=50;
           int height = 0;
           int width = 0;
   
           // Get the image from an S3 Bucket
           AmazonS3 s3client = AmazonS3ClientBuilder.defaultClient();
   
           com.amazonaws.services.s3.model.S3Object s3object = s3client.getObject(bucket, photo);
           S3ObjectInputStream inputStream = s3object.getObjectContent();
           BufferedImage image = ImageIO.read(inputStream);
           
           DetectCustomLabelsRequest request = new DetectCustomLabelsRequest()
           		.withProjectVersionArn(projectVersionArn)
                   .withImage(new Image().withS3Object(new S3Object().withName(photo).withBucket(bucket)))
                   .withMinConfidence(minConfidence);
   
           width = image.getWidth();
           height = image.getHeight();
   
           // Call DetectCustomLabels  
           AmazonRekognition amazonRekognition = AmazonRekognitionClientBuilder.defaultClient();
           DetectCustomLabelsResult result = amazonRekognition.detectCustomLabels(request);
           
           //Show the bounding box info for each custom label.
           List<CustomLabel> customLabels = result.getCustomLabels();
          
           for (CustomLabel customLabel : customLabels) {
               System.out.println(customLabel.getName());
   
           	if (customLabel.getGeometry()!=null)
               {
           		BoundingBox box = customLabel.getGeometry().getBoundingBox();
               
           		float left = width * box.getLeft();
           		float top = height * box.getTop();
           		System.out.println("Custom Label:");
   
           		System.out.println("Left: " + String.valueOf((int) left));
           		System.out.println("Top: " + String.valueOf((int) top));
           		System.out.println("Label Width: " + String.valueOf((int) (width * box.getWidth())));
           		System.out.println("Label Height: " + String.valueOf((int) (height * box.getHeight())));
           		System.out.println();
               }
   
           }
   
           // Create frame and panel.
           JFrame frame = new JFrame("CustomLabels");
           frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
           DetectCustomLabels panel = new DetectCustomLabels(result, image);
           panel.setPreferredSize(new Dimension(image.getWidth() / scale, image.getHeight() / scale));
           frame.setContentPane(panel);
           frame.pack();
           frame.setVisible(true);
   
       }
   }
   ```

------
#### [ Java 2 ]

   The following example code displays bounding boxes around custom labels detected in an image\. In the function `main`, replace the following values: 
   + `bucket` with the name of Amazon S3 bucket that you used in step 4\. 
   + `photo` with the name of the input image file you uploaded in step 4\.
   + `minConfidence` with the minimum confidence \(0\-100\)\. Objects detected with a confidence lower than this value are not returned\.
   + `projectVersionArn` with the ARN of the model that you want use with the input image\. 

**Note**  
To use a local image, uncomment the following lines in the fuction `main`\. Remember to set the value of `photo` to the path and filename of your local image\.  

   ```
   panel = new DetectCustomLabels(rekClient, projectVersionArn, photo, minConfidence);
   ```

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   package com.example.rekognition;
   
   import software.amazon.awssdk.core.ResponseBytes;
   import software.amazon.awssdk.core.SdkBytes;
   import software.amazon.awssdk.core.sync.ResponseTransformer;
   
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.S3Object;
   import software.amazon.awssdk.services.rekognition.model.Image;
   import software.amazon.awssdk.services.rekognition.model.DetectCustomLabelsRequest;
   import software.amazon.awssdk.services.rekognition.model.DetectCustomLabelsResponse;
   import software.amazon.awssdk.services.rekognition.model.CustomLabel;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   import software.amazon.awssdk.services.rekognition.model.BoundingBox;
   
   import software.amazon.awssdk.services.s3.S3Client;
   import software.amazon.awssdk.services.s3.model.GetObjectRequest;
   import software.amazon.awssdk.services.s3.model.GetObjectResponse;
   import software.amazon.awssdk.services.s3.model.NoSuchBucketException;
   
   import java.io.ByteArrayInputStream;
   import java.io.File;
   import java.io.FileInputStream;
   import java.io.FileNotFoundException;
   import java.io.IOException;
   import java.io.InputStream;
   import java.util.List;
   
   import java.awt.*;
   import java.awt.font.FontRenderContext;
   import java.awt.image.BufferedImage;
   import javax.imageio.ImageIO;
   import javax.swing.*;
   
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   // Calls DetectCustomLabels on an image. Displays bounding boxes or
   // image level labels found in the image.
   public class DetectCustomLabels extends JPanel {
   
       private static final long serialVersionUID = 1L;
       private transient BufferedImage image;
   
   
       // Holds response from DetectCustomLabels. Used to draw labels.
       private transient DetectCustomLabelsResponse response;
       private transient Dimension dimension;
       public static final Logger logger = Logger.getLogger(DetectCustomLabels.class.getName());
   
       // Finds custom labels in an image stored in an S3 bucket.
       public DetectCustomLabels(RekognitionClient rekClient, S3Client s3client, String projectVersionArn, String bucket,
               String key, Float minConfidence) throws RekognitionException, NoSuchBucketException, IOException {
           
           logger.log(Level.INFO, "Processing S3 bucket: {0} image {1}", new Object[] { bucket, key });
           // Get image from S3 bucket and create BufferedImage
           GetObjectRequest requestObject = GetObjectRequest.builder().bucket(bucket).key(key).build();
           ResponseBytes<GetObjectResponse> result = s3client.getObject(requestObject, ResponseTransformer.toBytes());
           ByteArrayInputStream bis = new ByteArrayInputStream(result.asByteArray());
           image = ImageIO.read(bis);
   
           // Set image size
           setWindowDimensions();
   
           // Construct request parameter for DetectCustomLabels
           S3Object s3Object = S3Object.builder().bucket(bucket).name(key).build();
   
           Image s3Image = Image.builder().s3Object(s3Object).build();
   
           DetectCustomLabelsRequest request = DetectCustomLabelsRequest.builder().image(s3Image)
                   .projectVersionArn(projectVersionArn).minConfidence(minConfidence).build();
   
           response = rekClient.detectCustomLabels(request);
           logFoundLabels(response.customLabels());
           drawLabels();
   
       }
   
   
   
       // Finds custom label in a local image file.
       public DetectCustomLabels(RekognitionClient rekClient, String projectVersionArn, String photo, Float minConfidence)
        throws IOException, RekognitionException
               {
   
               logger.log(Level.INFO, "Processing local file: {0}", photo);
               // Get image bytes and buffered image
               InputStream sourceStream = new FileInputStream(new File(photo));
               SdkBytes imageBytes = SdkBytes.fromInputStream(sourceStream);
               ByteArrayInputStream inputStream = new ByteArrayInputStream(imageBytes.asByteArray());
               image = ImageIO.read(inputStream);
   
   
               setWindowDimensions();
   
               // Construct request parameter for DetectCustomLabels
               Image localImageBytes = Image.builder().bytes(imageBytes).build();
   
               DetectCustomLabelsRequest request = DetectCustomLabelsRequest.builder().image(localImageBytes)
                       .projectVersionArn(projectVersionArn).minConfidence(minConfidence).build();
   
               response = rekClient.detectCustomLabels(request);
   
               logFoundLabels(response.customLabels());
               drawLabels();
   
       }
   
           //Sets window dimensions to 1/2 screen size, unless image is smaller
           public void setWindowDimensions(){
               dimension= java.awt.Toolkit.getDefaultToolkit().getScreenSize();
       
               dimension.width = (int) dimension.getWidth()/2;
               if (image.getWidth() < dimension.width) {
                   dimension.width = image.getWidth();
               }
               dimension.height = (int) dimension.getHeight()/2;
       
               if (image.getHeight()< dimension.height){
                   dimension.height = image.getHeight();
               }   
       
               setPreferredSize(dimension);
       
           }
   
       // Draws bounding boxes (if present) and label text.
       public void drawLabels() {
   
           int boundingBoxBorderWidth = 5;
           int imageHeight = image.getHeight(this);
           int imageWidth = image.getWidth(this);
   
           // Set up drawing
           Graphics2D g2d = image.createGraphics();
           g2d.setColor(Color.GREEN);
           g2d.setFont(new Font("Tahoma", Font.PLAIN, 30));
           Font font = g2d.getFont();
           FontRenderContext frc = g2d.getFontRenderContext();
           g2d.setStroke(new BasicStroke(boundingBoxBorderWidth));
   
           List<CustomLabel> customLabels = response.customLabels();
   
           int imageLevelLabelHeight = 0;
           for (CustomLabel customLabel : customLabels) {
   
               String label = customLabel.name();
   
               int textWidth = (int) (font.getStringBounds(label, frc).getWidth());
               int textHeight = (int) (font.getStringBounds(label, frc).getHeight());
   
               // Draw bounding box, if present
               if (customLabel.geometry() != null) {
   
                   BoundingBox box = customLabel.geometry().boundingBox();
                   float left = imageWidth * box.left();
                   float top = imageHeight * box.top();
   
                   // Draw black rectangle
                   g2d.setColor(Color.BLACK);
                   g2d.fillRect(Math.round(left + (boundingBoxBorderWidth)), Math.round(top + (boundingBoxBorderWidth)),
                           textWidth + boundingBoxBorderWidth, textHeight + boundingBoxBorderWidth);
   
                   // Write label onto black rectangle
                   g2d.setColor(Color.GREEN);
                   g2d.drawString(label, left + boundingBoxBorderWidth, (top + textHeight));
   
                   // Draw bounding box around label location
                   g2d.drawRect(Math.round(left), Math.round(top), Math.round((imageWidth * box.width())),
                           Math.round((imageHeight * box.height())));
               }
               // Draw image level labels.
               else {
                   // Draw black rectangle
                   g2d.setColor(Color.BLACK);
                   g2d.fillRect(10, 10 + imageLevelLabelHeight,
                       textWidth, textHeight);
                   g2d.setColor(Color.GREEN);
                   g2d.drawString(label, 10, textHeight+imageLevelLabelHeight);
   
                   imageLevelLabelHeight+=textHeight;
               }
   
           }
           g2d.dispose();
   
       }
       // Log the labels found by DetectCustomLabels
       private void logFoundLabels(List<CustomLabel> customLabels) {
           logger.info("Custom labels found");
           for (CustomLabel customLabel : customLabels) {
               logger.log(Level.INFO, " Label: {0}", customLabel.name());
               logger.log(Level.INFO, " Confidence: {0}", customLabel.confidence());
   
           }
       }
   
       // Draws the image containing the bounding boxes and labels.
       @Override
       public void paintComponent(Graphics g) {
   
           Graphics2D g2d = (Graphics2D) g; // Create a Java2D version of g.
   
           // Draw the image.
           g2d.drawImage(image, 0, 0, dimension.width, dimension.height, this);
   
       }
   
       public static void main(String arg[]) throws Exception {
   
           String photo = "photo";
           String bucket = "bucket";
           String projectVersionArn ="model_arn";
           float minConfidence = 50; 
   
           DetectCustomLabels panel = null;
   
           try{
               // Get the Rekognition client
               RekognitionClient rekClient = RekognitionClient
                   .builder()
                   .build();
               
               S3Client s3client = S3Client.builder() .build(); 
   
               // Create frame and panel.
               JFrame frame = new JFrame("Custom Labels");
               frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
   
               // Creating using image stored in S3 bucket.
               panel = new DetectCustomLabels(rekClient, s3client, projectVersionArn,bucket,photo,
               minConfidence);                 
   
   
               // Uncomment to call DetectCustomLabels with local file
               // Change photo to location of local image
               
               //panel = new DetectCustomLabels(rekClient, projectVersionArn, photo, minConfidence);
   
   
               frame.setContentPane(panel);
               frame.pack();
               frame.setVisible(true);
   
           
           } catch (RekognitionException rekError) {
               logger.log(Level.SEVERE, "Rekognition client error: {0}", rekError.getMessage());
               System.exit(1);
           } catch (FileNotFoundException fileError) {
               logger.log(Level.SEVERE, "File not found {0}", photo);
               System.exit(1);
           } catch (IOException fileError) {
               logger.log(Level.SEVERE, "Input output exception {0}", photo);
               System.exit(1);
           } catch (NoSuchBucketException fileError) {
               logger.log(Level.SEVERE, "Bucket not found: {0}", bucket);
               System.exit(1);
           } 
   
       }
   }
   ```

------

## DetectCustomLabels operation request<a name="detecttext-request"></a>

In the `DetectCustomLabels` operation, you supply an input image either as a base64\-encoded byte array or as an image stored in an Amazon S3 bucket\. The following example JSON request shows the image loaded from an Amazon S3 bucket\.

```
{
    "ProjectVersionArn": "string", 
     "Image":{ 
        "S3Object":{
            "Bucket":"string",
            "Name":"string",
            "Version":"string"
         }
    },
    "MinConfidence": 90,
    "MaxLabels": 10,
}
```

## DetectCustomLabels operation response<a name="text-response"></a>

The following JSON response from the `DetectCustomLabels` operation shows the custom labels that were detected in the following image\.

```
{
    "CustomLabels": [
        {
            "Name": "MyLogo",
            "Confidence": 77.7729721069336,
            "Geometry": {
                "BoundingBox": {
                    "Width": 0.198987677693367,
                    "Height": 0.31296101212501526,
                    "Left": 0.07924537360668182,
                    "Top": 0.4037395715713501
                }
            }
        }
    ]
}
```