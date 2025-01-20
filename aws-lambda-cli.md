# AWS Lambda CLI Command Cheatsheet

A comprehensive guide to AWS CLI commands for managing AWS Lambda functions and configurations.

## Table of Contents
- [Function Management](#function-management)
- [Function Configuration](#function-configuration)
- [Function Versions and Aliases](#function-versions-and-aliases)
- [Permissions and IAM](#permissions-and-iam)
- [Event Source Mapping](#event-source-mapping)
- [Monitoring and Logs](#monitoring-and-logs)

## Function Management

### Create a Lambda Function
```bash
# Create function from zip file
aws lambda create-function \
    --function-name my-function \
    --runtime python3.9 \
    --role arn:aws:iam::123456789012:role/lambda-role \
    --handler app.lambda_handler \
    --zip-file fileb://function.zip

# Create function from S3
aws lambda create-function \
    --function-name my-function \
    --runtime python3.9 \
    --role arn:aws:iam::123456789012:role/lambda-role \
    --handler app.lambda_handler \
    --code S3Bucket=my-bucket,S3Key=function.zip
```

### Update Function Code
```bash
# Update from zip file
aws lambda update-function-code \
    --function-name my-function \
    --zip-file fileb://function.zip

# Update from S3
aws lambda update-function-code \
    --function-name my-function \
    --s3-bucket my-bucket \
    --s3-key function.zip
```

### Invoke Function
```bash
# Synchronous invocation
aws lambda invoke \
    --function-name my-function \
    --payload '{"key": "value"}' \
    response.json

# Asynchronous invocation
aws lambda invoke \
    --function-name my-function \
    --invocation-type Event \
    --payload '{"key": "value"}' \
    response.json
```

### List and Delete Functions
```bash
# List functions
aws lambda list-functions

# Delete function
aws lambda delete-function \
    --function-name my-function
```

## Function Configuration

### Update Function Configuration
```bash
# Update memory and timeout
aws lambda update-function-configuration \
    --function-name my-function \
    --memory-size 256 \
    --timeout 30

# Update environment variables
aws lambda update-function-configuration \
    --function-name my-function \
    --environment "Variables={KEY1=value1,KEY2=value2}"

# Update runtime
aws lambda update-function-configuration \
    --function-name my-function \
    --runtime python3.9
```

### Get Function Configuration
```bash
aws lambda get-function-configuration \
    --function-name my-function
```

## Function Versions and Aliases

### Publish Version
```bash
aws lambda publish-version \
    --function-name my-function \
    --description "Version description"
```

### Create Alias
```bash
aws lambda create-alias \
    --function-name my-function \
    --name prod \
    --function-version 1

# Create alias with routing config
aws lambda create-alias \
    --function-name my-function \
    --name prod \
    --function-version 1 \
    --routing-config AdditionalVersionWeights={"2"=0.1}
```

### Update Alias
```bash
aws lambda update-alias \
    --function-name my-function \
    --name prod \
    --function-version 2
```

### List Versions and Aliases
```bash
# List versions
aws lambda list-versions-by-function \
    --function-name my-function

# List aliases
aws lambda list-aliases \
    --function-name my-function
```

## Permissions and IAM

### Add Permission
```bash
# Add permission for S3
aws lambda add-permission \
    --function-name my-function \
    --statement-id s3-trigger \
    --action lambda:InvokeFunction \
    --principal s3.amazonaws.com \
    --source-arn arn:aws:s3:::my-bucket

# Add permission for API Gateway
aws lambda add-permission \
    --function-name my-function \
    --statement-id api-gateway \
    --action lambda:InvokeFunction \
    --principal apigateway.amazonaws.com \
    --source-arn "arn:aws:execute-api:region:account-id:api-id/*/*/*"
```

### Remove Permission
```bash
aws lambda remove-permission \
    --function-name my-function \
    --statement-id s3-trigger
```

### Get Policy
```bash
aws lambda get-policy \
    --function-name my-function
```

## Event Source Mapping

### Create Event Source Mapping
```bash
# Create DynamoDB mapping
aws lambda create-event-source-mapping \
    --function-name my-function \
    --event-source-arn arn:aws:dynamodb:region:account-id:table/my-table/stream/timestamp \
    --batch-size 100 \
    --starting-position LATEST

# Create SQS mapping
aws lambda create-event-source-mapping \
    --function-name my-function \
    --event-source-arn arn:aws:sqs:region:account-id:my-queue \
    --batch-size 10
```

### Update Event Source Mapping
```bash
aws lambda update-event-source-mapping \
    --uuid "uuid-from-create-command" \
    --batch-size 200
```

### List and Delete Event Source Mappings
```bash
# List mappings
aws lambda list-event-source-mappings \
    --function-name my-function

# Delete mapping
aws lambda delete-event-source-mapping \
    --uuid "uuid-from-create-command"
```

## Monitoring and Logs

### Get Function Metrics
```bash
aws cloudwatch get-metric-statistics \
    --namespace AWS/Lambda \
    --metric-name Invocations \
    --dimensions Name=FunctionName,Value=my-function \
    --start-time "2024-01-13T00:00:00" \
    --end-time "2024-01-20T00:00:00" \
    --period 3600 \
    --statistics Sum
```

### Get Function Logs
```bash
# Get log streams
aws logs describe-log-streams \
    --log-group-name /aws/lambda/my-function

# Get log events
aws logs get-log-events \
    --log-group-name /aws/lambda/my-function \
    --log-stream-name "stream-name"
```

## Tips and Best Practices

1. Always test Lambda functions locally before deployment
2. Use environment variables for configuration
3. Set appropriate memory and timeout values
4. Implement proper error handling
5. Use aliases for production deployments
6. Monitor function metrics and logs
7. Clean up unused functions and versions
8. Use appropriate batch sizes for event source mappings
9. Implement retries for asynchronous invocations
10. Follow the principle of least privilege for IAM roles

## Contributing

Feel free to submit issues and enhancement requests!

## License

This project is licensed under the MIT License - see the LICENSE file for details.
