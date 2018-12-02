# ansible-role-aws-iam

Ansible role to create arbitrary numbers of AWS IAM groups, users, roles and policies

## Requirements

* Ansible >= 2.5
* The AWS CLI must be installed in the environment where this will run.

## Additional variables

Additional variables that can be used (either as `host_vars`/`group_vars` or via command line args):

| Variable                 | Description                  |
|--------------------------|------------------------------|
| `aws_iam_profile`        | Boto profile name to be used |
| `aws_iam_default_region` | Default region to use        |

# Example definition

#### Required parameter only

#### Create user(s)

```yml
aws_iam_iamuser:
  - name: a_sample_user1
  - name: a_sample_user2
```

#### Create role(s)

```yml
aws_iam_iamrole:
  - name: a_sample_role1
    description: 'A test role'
    assume_role_policy_document: "{{ lookup('file','policy.json') }}"
  - name: a_sample_role2
    description: 'Another test role'
    assume_role_policy_document: "{{ lookup('file','policy2.json') }}"
```

#### Create group(s)

```yml
aws_iam_iamgroup:
  - name: a_sample_group1
    users:
      - user1
      - user2
      - user3
  - name: a_sample_group2
```

#### Create policy(ies)

```yml
aws_iam_iampolicy:
  - name: a_sample_policy1
    iam_name: an_iam_user
    policy_json: " {{ lookup( 'template', 's3_policy.json.j2') }} "
    iam_type: user
  - name: a_sample_policy2
    iam_name: an_iam_group
    policy_json: " {{ lookup( 'template', 'es_policy.json.j2') }} "
    iam_type: group
```

#### Create identity provider(s)

```yml
aws_iam_saml_provider:
  - name: a_sample_saml_provider
    iam_name: saml_provider_example
    metadata_document: " {{ lookup( 'template', 'saml_metadata.xml.j2') }} "
```

#### All available parameter
```yml
aws_iam_iamuser:
  - name: an_iam_user
    # Optional
    region: eu-central-1
    tags:
      - key: env
        val: development
      - key: department
        val: infra
  - name: an_iam_user2
    # Optional
    region: eu-central-1
    tags:
      - key: env
        val: production
      - key: department
        val: devops
```

## Testing

#### Requirements

* Docker
* [yamllint](https://github.com/adrienverge/yamllint)

#### Run tests

```bash
# Lint the source files
make lint
# Run integration tests with default Ansible version
make test

# Run integration tests with custom Ansible version
make test ANSIBLE_VERSION=2.5
```
