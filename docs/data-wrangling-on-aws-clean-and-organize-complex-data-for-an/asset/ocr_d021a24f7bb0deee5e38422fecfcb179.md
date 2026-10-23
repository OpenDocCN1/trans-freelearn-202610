```
```
            output=output+"------------------------------------------------------\n" +
            event ["query" ] [i] +"\n "
            i=i+1
            lines = csv.reader(data)
            headers = next(lines)
            output=output + "Header "+ ",".join(headers)+"\n"
            for line in lines:
                output=output+",".join(line)+"\n "
            return output
```
```

**Step 3d**: Update the IAM role attached to the Lambda function to have Amazon S3 permissions. We will attach an AmazonS3FullAccess managed policy with the IAM role. In a production setup, provide access only to the needed S3 location to further restrict access.

**Step 3e**: Search for Lambda in the step function console and pull that into our workflow after the pass step. Enter the function name parameter with the Lambda function that was created in *step 3b*, as illustrated here:

![](bbox=[0.15, 0.4, 0.86, 0.57])

Figure 7.49: Step function workflow – Lambda function to consolidate the results from the Athena query execution

**Step 3f**: Filter the output path to fetch only the Lambda output to be sent in an email through an SNS topic. In the Output tab, choose Filter output with OutputPath and enter `$ . Payload`.

**Step 4**: Send the consolidated results through email.

**Step 4a**: Navigate to the SNS console in a separate tab and create an SNS topic with a **Standard** topic type. Once the topic is created, click on the topic, and create a subscription from the topic. Select **Email** as the protocol and enter your email address (or a group) in the **Endpoint** field. Ensure that you click on the confirm subscription email.

**Step 4b**: Move back to the step function workflow and search for SNS `publish`. Drag the SNS Publish API icon to the workflow after the Lambda function.