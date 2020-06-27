# parallelcluster-closednetwork
set up AWS ParallelCluster on closed network environment


## Usage

### 1. Set up VPC and Private Gateways

#### 1a: Set up with CloudFormation

[![Launch](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?#/stacks/new?stackName=ClosedEnvironment&templateURL=https://midaisuk-public-templates.s3.amazonaws.com/parallelcluster-closednetwork/closed-vpc-privatelink.yml
)

or with CLI

```bash
$ aws cloudformation create-stack --stack-name ClosedEnvironment --template-url https://midaisuk-public-templates.s3.amazonaws.com/parallelcluster-closednetwork/closed-vpc-privatelink.yml
```

#### 1b: Set up manually

You need to set up these private gateways for private VPC.

- s3
- dynamodb
- logs
- cloudformation
- monitoring
- ec2
- sqs
- sns
- autoscaling

### 2. Launch AWS ParallelCluster

Launch ParallelCluster with config file for closed network condition.

- closed-network.config

```
[aws]
aws_region_name = <REGION>

[global]
update_check = true
sanity_check = true
cluster_template = closed

[aliases]
ssh = ssh {CFN_USER}@{MASTER_IP} {ARGS}

[cluster closed]
key_name = <KEY_NAME>
additional_iam_policies = arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
base_os = alinux2
scheduler = slurm
master_instance_type = c5.large
compute_instance_type = c5.xlarge
disable_hyperthreading = true
initial_queue_size = 0
max_queue_size = 10
vpc_settings = closed

[vpc closed]
vpc_id = <VPC_ID>
master_subnet_id = <SUBNET_ID>
use_public_ips = false
```

You should change `<REGION>`, `<KEY_NAME>`, `VPC_ID`, `SUBNET_ID`.
You could find `VPC_ID`, `SUBNET_ID` in the output of the cloudformation.

```
$ pcluster create -c closed-network.config closed-cluster
```

### 3. Connect the cluster by Session Manager

```
$ sudo su ec2-user
$ cd
$ cat > job.sh
#!/bin/bash
hostname
$ sbatch job.sh
```

## Cost

You need to have extra cost for PrivateLink.


