# Devopslambda
terraform 


We're ready to initialize terraform and apply it.


terraform init
terraform apply
Since we don't have API Gateway just yet, let's invoke this function with the aws lambda invoke-command.


aws lambda invoke --region=us-east-1 --function-name=hello response.json
If you print the response, you should see the message Hello World!


cat response.json


Go back to the terminal and apply the terraform.


terraform apply
Let's test the HTTP GET method first, append the hello endpoint and optionally provide the URL parameter.


curl "https://<id>.execute-api.us-east-1.amazonaws.com/dev/hello?Name=Anton"
Also, let's test the POST method. In this case, we provide a payload as a JSON object to the same hello endpoint.


curl -X POST \
-H "Content-Type: application/json" \
-d '{"name":"Anton"}' \
"https://<id>.execute-api.us-east-1.amazonaws.com/dev/hello"
If you just want to deploy it from your local machine, you can create a simple wrapper around terraform. This would be an extremely simple script, just to give you an idea of how to build and deploy it.

terraform/terraform.sh

#!/bin/sh

set -e

cd ../s3
npm ci

cd ../terraform
terraform apply
Let's make this script executable.


chmod +x terraform.sh
Finally, let's run it.


./terraform.sh
We can invoke this new s3 function and provide a json payload with the bucket name and the object.


aws lambda invoke \
--region=us-east-1 \
--function-name=s3 \
--cli-binary-format raw-in-base64-out \
--payload '{"bucket":"test-<your>-<name>","object":"hello.json"}' \
response.json
