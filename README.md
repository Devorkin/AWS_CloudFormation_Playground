# My CloudFormation Playground project

## Dependencies

1. Set AWS authentication creds as your Shell variables

    ```bash
    export export AWS_ACCESS_KEY_ID='<AWS ACCESS ID HERE>'
    export AWS_SECRET_ACCESS_KEY='<AWS SECRET ACCESS KEY HERE>'
    ```

2. Import SSHkeys you would like to use with AWS EC2 instances

    ```bash
    aws ec2 import-key-pair --key-name MyKeyPair --public-key-material "$(cat $HOME/.ssh/${PUBLIC_SSH_KEY_FILENAME}.pub -p | base64 | tr -d '\n')"
    ```

3. Check for the Default VPC ID and Subnet ID you would like to use with this CF template

    ```bash
    aws ec2 describe-subnets --query "Subnets[*].[SubnetId, VpcId, AvailabilityZone, CidrBlock]" --output table
    ```

    Note: Choose just a single Subnet ID

## Apply the CloudFormation template

```bash
aws cloudformation update-stack --stack-name ${STACK_NAME} \n
    --parameters ParameterKey=DefaultVPCID,ParameterValue=${VPC_ID} \n
        ParameterKey=DefaultSubnetID,ParameterValue=${SUBNET_ID} \n
        ParameterKey=ResourceTitleSuffix,ParameterValue=${RESOURCE_TITLE_SUFFIX} \n
    --tags Key=Build,Value=Alpha \n
    --template-body file://modified-demo-template-init.yaml
```

Enjoy.
