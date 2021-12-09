# Listing dataset entries \(SDK\)<a name="md-listing-dataset-entries-sdk"></a>

You can use the `ListDatasetEntries` API to list the JSON lines for each image in a dataset\. For more information, see [Creating a manifest file](md-create-manifest-file.md)\.

**To list dataset entries \(SDK\)**

1. If you haven't already:

   1. Create or update an IAM user with `AmazonRekognitionFullAccess` permissions\. For more information, see [Step 2: Create an IAM administrator user and group](su-account-user.md)\.

   1. Install and configure the AWS CLI and the AWS SDKs\. For more information, see [Step 3: Set Up the AWS CLI and AWS SDKs](su-awscli-sdk.md)\.

1. Use the following example code list the entries in a dataset

------
#### [ AWS CLI ]

   Change the value of `dataset-arn` to the ARN of the dataset that you want to list\.

   ```
   aws rekognition list-dataset-entries --dataset-arn dataset_arn 
   ```

   To list only JSON lines with errors, specify `has-errors`\.

   ```
   aws rekognition list-dataset-entries --dataset-arn dataset_arn \
   --has-errors
   ```

------
#### [ Python ]

   Use the following code\. Supply the following command line parameters:
   + dataset\_arn — the ARN of the dataset that you want to list\.
   + show\_errors\_only — specify `true` if you want to see errors only\. `false` otherwise\.

   ```
   # Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   # PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   
   import boto3
   import argparse
   import logging
   
   from botocore.exceptions import ClientError
   
   logger = logging.getLogger(__name__)
   
   
   def list_dataset_entries(rek_client, dataset_arn, show_errors):
       """
       Lists the entries in an Amazon Rekognition Custom Labels dataset.
       :param rek_client: The Amazon Rekognition Custom Labels Boto3 client.
       :param dataset_arn: The ARN of the dataet that you want to use.
       """
   
       try:
           # List the entries
           logger.info(f"Listing dataset entries for the dataset {dataset_arn}.")
   
           finished = False
           count=0
           next_token = ""
           show_errors_only = False
   
           if show_errors.lower() == "true":
               show_errors_only=True
   
   
           while finished == False:
   
               response = rek_client.list_dataset_entries(
                   DatasetArn=dataset_arn,
                   HasErrors=show_errors_only,
                   MaxResults=100,
                   NextToken=next_token)
   
               count += len(response['DatasetEntries'])
   
               for entry in response['DatasetEntries']:
                   print(entry)
   
               if not 'NextToken' in response:
                   finished = True                
                   logger.info(f"No more entries. Total:{count}")
               else:
                   next_token=next_token=response['NextToken']
                   logger.info(f"Getting more entries. Total so far :{count}")
                   
   
       except ClientError as err:
           logger.exception(
               f"Couldn't list dataset: {err.response['Error']['Message']}")
           raise
   
   
   def add_arguments(parser):
       """
       Adds command line arguments to the parser.
       :param parser: The command line parser.
       """
   
       parser.add_argument(
           "dataset_arn", help="The ARN of the dataset that you want to list."
          
       )
   
       parser.add_argument(
       "show_errors_only", help="true if you want to see errors only. false otherwise."
       )
   
   
   def main():
   
       logging.basicConfig(level=logging.INFO,
                           format="%(levelname)s: %(message)s")
   
       try:
   
           # get command line arguments
           parser = argparse.ArgumentParser(usage=argparse.SUPPRESS)
           add_arguments(parser)
           args = parser.parse_args()
   
           print(f"Listing entries for  dataset {args.dataset_arn}")
   
           # List the dataset entries
           rek_client=boto3.client('rekognition')
   
           list_dataset_entries(rek_client,
               args.dataset_arn,
               args.show_errors_only)
   
           print(f"Finished listing entries for dataset: {args.dataset_arn}")
   
       except ClientError as err:
           logger.exception(f"Problem listing dataset: {err}")
           print(f"Problem listing dataset: {err}")
       except Exception as err:
           logger.exception(f"Problem listing dataset: {err}")
           print(f"Problem listing dataset: {err}")
   
   
   if __name__ == "__main__":
       main()
   ```

------
#### [ Java 2 ]

   Use the following code\. Supply the following command line parameters:
   + dataset\_arn — the ARN of the dataset that you want to list\.
   + show\_errors\_only — specify `true` if you want to see errors only\. `false` otherwise\.

   ```
   //Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.
   //PDX-License-Identifier: MIT-0 (For details, see https://github.com/awsdocs/amazon-rekognition-custom-labels-developer-guide/blob/master/LICENSE-SAMPLECODE.)
   
   
   import software.amazon.awssdk.services.rekognition.RekognitionClient;
   import software.amazon.awssdk.services.rekognition.model.ListDatasetEntriesRequest;
   import software.amazon.awssdk.services.rekognition.model.RekognitionException;
   import software.amazon.awssdk.services.rekognition.paginators.ListDatasetEntriesIterable;
   
   
   import java.net.URI;
   import java.util.logging.Level;
   import java.util.logging.Logger;
   
   public class ListDatasetEntries {
   
       public static final Logger logger = Logger.getLogger(ListDatasetEntries.class.getName());
   
       public static void listMyDatasetEntries(RekognitionClient rekClient, String datasetArn, boolean showErrorsOnly)
               throws Exception, RekognitionException {
   
           try {
   
               logger.log(Level.INFO, "Listing dataset {0}", new Object[] { datasetArn });
   
               ListDatasetEntriesRequest listDatasetEntriesRequest = ListDatasetEntriesRequest.builder()
                       .hasErrors(showErrorsOnly).datasetArn(datasetArn).maxResults(1).build();
   
               ListDatasetEntriesIterable datasetEntriesList = rekClient
                       .listDatasetEntriesPaginator(listDatasetEntriesRequest);
   
               datasetEntriesList.stream().flatMap(r -> r.datasetEntries().stream())
                       .forEach(datasetEntry -> System.out.println(datasetEntry.toString()));
   
           } catch (RekognitionException e) {
               logger.log(Level.SEVERE, "Could not update dataset: {0}", e.getMessage());
               throw e;
           }
   
       }
   
       public static void main(String args[]) {
   
           boolean showErrorsOnly = false;
           String datasetArn = null;
   
           final String USAGE = "\n" + "Usage: " + "<project_arn> <dataset_arn> <updates_file>\n\n" + "Where:\n"
                   + "   dataset_arn - the ARN of the dataset that you want to update.\n\n"
                   + "   show_errors_only - true to show only errors. false otherwise.\n\n";
   
           if (args.length != 2) {
               System.out.println(USAGE);
               System.exit(1);
           }
   
           datasetArn = args[0];
           if (args[1].toLowerCase().equals("true")) {
   
               showErrorsOnly = true;
           }
   
           try {
   
               // Get the Rekognition client
               RekognitionClient rekClient = RekognitionClient.builder().build();
   
                // list the dataset
   
               listMyDatasetEntries(rekClient, datasetArn, showErrorsOnly);
   
               System.out.println(String.format("Finished listing entries for : %s", datasetArn));
   
               rekClient.close();
   
           } catch (RekognitionException rekError) {
               logger.log(Level.SEVERE, "Rekognition client error: {0}", rekError.getMessage());
               System.exit(1);
           } catch (Exception rekError) {
               logger.log(Level.SEVERE, "Error: {0}", rekError.getMessage());
               System.exit(1);
           }
   
       }
   
   }
   ```

------