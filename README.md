## Links
see here https://sysdig.com/blog/deploy-openshift-aws/


1. copy cloudformation.yml to s3

## create stack
```
aws cloudformation create-stack \
 --region eu-central-1 \
 --stack-name sysdig-origin-1 \
 --template-url "https://s3.eu-central-1.amazonaws.com/sysdig-cloudformation/CloudFormationTemplateOpenShift.yaml" \
 --parameters \
   ParameterKey=AvailabilityZone,ParameterValue=eu-central-1a \
   ParameterKey=KeyName,ParameterValue=ansible-eu-central-1 \
 --capabilities=CAPABILITY_IAM
```

## update stack (optional)
```
aws cloudformation update-stack \
 --region eu-central-1 \
 --stack-name sysdig-origin-1 \
 --template-url "https://s3.eu-central-1.amazonaws.com/sysdig-cloudformation/CloudFormationTemplateOpenShift.yaml" \
 --parameters \
   ParameterKey=AvailabilityZone,ParameterValue=eu-central-1a \
   ParameterKey=KeyName,ParameterValue=ansible-eu-central-1 \
 --capabilities=CAPABILITY_IAM
```
## delete stack

`aws cloudformation delete-stack --stack-name=sysdig-origin-1`

## ssh access

`ssh -i ./ssh_keys/ansible-eu-central-1 centos@<AWS_HOST_IP>`

## prepare (for vagrant use only)
* `sudo chown -R vagrant /tmp`
* distribute ssh key to nodes
* add host entries to `/etc/hosts
`
## prepare (for aws use only)
* `ansible-playbook prepare.yml -i hosts.contiv --key-file ./ssh_keys/ansible-eu-central-1`

## run playbook

`ansible-playbook -c paramiko -i hosts.contiv openshift-ansible/playbooks/byo/config.yml --key-file ./ssh_keys/ansible-eu-central-1`