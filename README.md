# AWS_Lambda_APIGateway_FileUpload
## API Gateway set up
- Python lambda function
```
import json
import boto3
import base64
from botocore.vendored import requests

def lambda_handler(event, context):
    s3 = boto3.resource('s3')
    bucket = s3.Bucket('aldiuat')
    path_test = '/tmp/output'         # temp path in lambda.
    key = event['name']          # assign filename to 'key' variable
    print("key=" + key)
    data = event['base64Image']             # assign base64 of an image to data variable 
    print("data=" + data)
    data1 = data
    img = base64.b64decode(data1)     # decode the encoded image data (base64)
    # res = base64.b64encode('Done Image Uploaded'.encode('utf-8'))
    with open(path_test, 'wb') as data:
        # data.write(data1)
        data.write(img)
        bucket.upload_file(path_test, "test-upload/" + key)   # Upload image directly inside bucket
        #bucket.upload_file(path_test, 'FOLDERNAME-IN-YOUR-BUCKET /{}'.format(key))    # Upload image inside folder of your s3 bucket.
    print('res---------------->',path_test)
    print('key---------------->',key)

    return {
        'isBase64Encoded': False,
        'headers': {
            'Content-Type': 'application/json'
        },
        'statusCode': 200,
        'body':  json.dumps({
            "content": "Image uploaed",
        })
    }
```
A Python demo for uploading a file onto AWS S3 via APIGateway and Lambda
