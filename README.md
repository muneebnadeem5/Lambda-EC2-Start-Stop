Code to take snapshot

























# Lambda-EC2-Auto Snapshot

Take-SnapShot
-------------
import json
import boto3

def lambda_handler(event, context):
    # TODO implement
    # create client for ec2 service and pass aws_access_key_id and aws_secret_access_key as parameter
    client = boto3.client('ec2', region_name='eu-central-1', aws_access_key_id= 'AKIAR5JLW6BQ3LIBHHAO',
                              aws_secret_access_key= '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' )
    # create a dictionary with names of databases or instances as key and their volume ids as value
    volumes_dict = {
                    'new-operations-dashboard-ksa-prod': 'vol-054c3f17bfd33eddc',
                    'KSA-Micro-Services-Node-Production': 'vol-026d5242fc8e23556',
                    'KSA-Resto-Panel-Prod': 'vol-060fc301b093e37c8',
                    'KSA-Admin-Production': 'vol-0105bc9f2200ac988',
                    'KSA-Web-Production': 'vol-080d1743721a8fe58',
                    'KSA-Catering / Kitopi-Prod': 'vol-095234dba9a0bc817',
                    'springms-ksa-onbarding-prod': 'vol-02e2ba5189ec6a37e',
                    'springms-ksa-gateway-registry-prod': 'vol-0c3c18409784c03ad',
                    'sprinms-ksa-user-prod': 'vol-01d233f6431e04c56',
                    'springms-ksa-hybrid-prod': 'vol-010814323c0f3103a',
                    'springms-ksa-menu-prod': 'vol-0c0179a1d8d7066e1',
                    'springms-ksa-order-prod': 'vol-0f526cde246c639c4',
                    'springms-ksa-sub-prod': 'vol-0b0beeb294246a1f4',
                    'Munchon-DriverApp-prod': 'vol-0be1c8a05aa81b35f',
                    'Munchon-DriverApp-prod2': 'vol-0ee91022d2cbc6251'
                  }

    successful_snapshots = dict()
    # iterate through each item in volumes_dict and use key as description of snapshot
    for snapshot in volumes_dict:
        try:
            print('Taking SnapShot :%s' % snapshot)
            response = client.create_snapshot(
                Description= snapshot,
                VolumeId= volumes_dict[snapshot],
                DryRun= False,
            )
            # response is a dictionary containing ResponseMetadata and SnapshotId
            status_code = response['ResponseMetadata']['HTTPStatusCode']
            snapshot_id = response['SnapshotId']
            # check if status_code was 200 or not to ensure the snapshot was created successfully
            if status_code == 200:
                successful_snapshots[snapshot] = snapshot_id
        except Exception as e:
            exception_message = "There was error in creating snapshot " + snapshot + " with volume id "+volumes_dict[snapshot]+" and error is: \n"\
                                + str(e)
    # print the snapshots which were created successfully
    print('SnapShot has been Scheduled : %s' % successful_snapshots)

    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
