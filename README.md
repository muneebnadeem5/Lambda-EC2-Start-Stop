# Lambda-EC2-Start-Stop
Lambda-EC2-Start &amp; Stop


**Start:**
------
import boto3
region = 'eu-central-1' 
instances = ['i-099b5a7e9a4248cbf']                         \\ Instance-ID
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.start_instances(InstanceIds=instances)
    print('started your instances: ' + str(instances))


**Stop**
import boto3
region = 'eu-central-1'
instances = ['i-099b5a7e9a4248cbf']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('stopped your instances: ' + str(instances))
