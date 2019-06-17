# Role-name

Associate a private Route53 Zone across accounts.

## Requirements

Since this is a cross account operation - credentials for each account are required (or an acocunt with the correct assume_role permissions to perform the actions in both accounts)

1. AWS credentials and the correct permissions to create the resources in the account with the Route53 zone
2. AWS credentials and the correct permissions to associate the Zone with a VPC in the target account.

## Role Variables

The variables uses in this role are

| Variable Name | Required | Description | 
|----|----|----|
| `associate_vpc_region`| **Yes** | The region of the VPC that is going to be attached to the zone |
| `associate_vpc_id`| **Yes** | The ID of the VPC that is going to be attached to the zone |
| `associate_zone_id`| **Yes** | The ID of the Route53 private zone to be used |

## Dependencies

AWS CLI is used - and must be installed. (tested with v1.16.179)

## Example Playbook

### Download dependencies

#### Create requirements file

Create a `requirements.yml` file with the following contents
```
- src: https://github.com/maishsk/route53-associate-zone
  version: master
```

#### Download dependencies
Run the following command:
```
ansible-galaxy install -r requirements.yml --force -p .
```

### Create playbook
Create a `main.yaml` file with the following contents:
```
---
- name: Example playbook
  hosts: localhost
  connection: local
  gather_facts: false
  vars_files:
    - vars/vars.yml

  tasks:
  - name: Create Process
    include_role:
      name: "{{ item }}"
    with_items:
      - route53-associate-zone
    tags: [ 'never', 'create' ]

  - name: Rollback Process
    include_role:
      name: "{{ item }}"
    with_items:
      - route53-associate-zone
    tags: [ 'never', 'rollback' ]
```

Create a `vars/vars.yml` with the content similar to:
```
associate_vpc_region: us-east-2
associate_vpc_id: vpc-12345678
associate_zone_id: Z01330002BD4XQ5W12345
```

## Running the playbook

To create the zone association 

`ansible-playbook main.yml --tags create`

To remove the zone association

`ansible-playbook main.yml --tags rollback`

## License

BSD

## Author Information
This role was created by [Maish Saidel-Keesing](https://www.maishsk.com/), author of [The Cloud Walkabout](http://cloudwalkabout.com/).