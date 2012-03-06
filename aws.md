---
title: AWS / EC2
layout: wikistyle
---

### Patterns

Python boto - auto scaling
--------------------


see: https://gist.github.com/958833

    collectorTierScalingGroup = AutoScalingGroup(name='ctScalingGroup',
                                                 availability_zones=zoneStrings,
                                                 default_cooldown=300,
                                                 desired_capacity=2,
                                                 min_size=2,
                                                 max_size=6,
                                                 load_balancers=[collectorLoadBalancerName],
                                                 launch_config=collectorTierLaunchConfig)
    
    as_con.create_auto_scaling_group(collectorTierScalingGroup)
    
    # Now create the scaling conditions, under which the scaling will take place
    # This is a two-stage process: create a scaleUp and scaleDown policy, and then tie these to metrics
    # which we will fetch using the CloudWatch monitoring services
    
    collectionTierScalingUpPolicy = ScalingPolicy(name='ctScaleUp',
                                                  adjustment_type='ChangeInCapacity',
                                                  as_name=collectorTierScalingGroup.name,
                                                  scaling_adjustment=2,
                                                  cooldown=180)
    
    collectionTierScalingDownPolicy = ScalingPolicy(name='ctScaleDown',
                                                  adjustment_type='ChangeInCapacity',
                                                  as_name=collectorTierScalingGroup.name,
                                                  scaling_adjustment=-1,
                                                  cooldown=180)
    
    as_con.create_scaling_policy(collectionTierScalingUpPolicy)
    as_con.create_scaling_policy(collectionTierScalingDownPolicy)


### Boto - Removing an AWS alarm by instance ID:

Remove alarms associated with an instance id

    def remove_alarms(self, instanceid):
        if not isinstance(instanceid, str):
            instanceid = instanceid.id
        a = [a.name for a in conn.cw.describe_alarms() if hasattr(a, 'dimensions')
                            and instanceid in a.dimensions.get('InstanceId', [])]
        print ('Found %s' %a)
        conn.cw.delete_alarms(a)



### Giving Permission S3 Objects via ACLs

Use the following JSON snippet to give access to users to a specific S3 bucket folder on AWS:


      {
      "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "s3:ListBucket",
          "s3:ListBucketMultipartUploads",
          "s3:ListBucketVersions",
      "s3:GetBucketAcl",
          "s3:GetBucketLocation",
          "s3:GetBucketNotification"
        ],
        "Resource": "arn:aws:s3:::MYBUCKET",
        "Condition": {
          "StringLike": {
            "s3:prefix": "folder/*"
          }
        }
      }, 
    
      {
        "Effect": "Allow",
        "Action": [
          "s3:AbortMultipartUpload",
          "s3:DeleteObject",
          "s3:ListMultipartUploadParts",
          "s3:PutObject",
          "s3:GetObjectAcl",
          "s3:GetObjectVersionAcl",
          "s3:PutObjectAcl",
          "s3:PutObjectAclVersion"
        ],
        "Resource": "arn:aws:s3:::MYBUCKET/folder/*"
      }
    ]
    }

