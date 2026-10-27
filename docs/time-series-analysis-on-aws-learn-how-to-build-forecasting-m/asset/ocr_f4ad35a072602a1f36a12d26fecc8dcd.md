2. On the **Define state machine** screen, select **Write your workflow in code** at the top and select the **Standard** type.

![](bbox=[0.115, 0.145, 0.87, 0.465])

Figure 12.25 – State machine definition

3. Download the following file:
https://packt-publishing-timeseries-on-aws-michaelhoarau.s3.eu-west-1.amazonaws.com/part2-amazon-lookout-for-equipment/lookout-equipment-automation-step-function.json

4. Open the preceding text file and search for the following string: <<LAMBDA ARN>>. Replace this string with the ARN of the Lambda function you copied previously (at the end of the Lambda function definition).

5. Copy the whole content of your text file (with the updated Lambda function ARN) into the **Definition** area of your state function. The workflow on the right is updated with the actual flow of the state machine.