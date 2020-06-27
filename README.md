# parallelcluster-closednetwork
set up AWS ParallelCluster on closed network environment


## Usage

### Set up VPC and Private Gateways

#### Set up with CloudFormation

[![Launch](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?#/stacks/new?stackName=ClosedEnvironment&templateURL=https://midaisuk-public-templates.s3.amazonaws.com/parallelcluster-closednetwork/closed-vpc-privatelink.yml
)

or with CLI

```bash
$ aws cloudformation create-stack --stack-name ClosedEnvironment --template-url https://midaisuk-public-templates.s3.amazonaws.com/parallelcluster-closednetwork/closed-vpc-privatelink.yml
```

#### Set up manually

### Launch AWS ParallelCluster

### Connect the cluster by Session Manager



