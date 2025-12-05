# aws-win-env

This playbook is intended to provide an easily deployable Windows instance in AWS for testing DLS in an environment that mimics enterprise environment limitations such as:
- Non-Admin user
- HTTP/2 disabled
- WDAC Profiles and AppLocker (TBD)

## Prerequisites
If on Fedora:
```
sudo dnf -y install ansible python3-winrm python3-boto3
```

On other platforms:
```
pip install ansible pywinrm
ansible-galaxy collection install amazon.aws
```

## Usage:
```
ansible-playbook ./ansible-windows-extension.yml \
  -e aws_access_key_id=$AWS_ACCESS_KEY_ID \
  -e aws_secret_access_key=$AWS_SECRET_ACCESS_KEY \
  -e gh_token=$GH_TOKEN \
  -e action=deploy \
  -e guid=$USER-win2025 \
  -e disable_http2=true
```

## To clean up
```
ansible-playbook ./ansible-windows-extension.yml \
  -e aws_access_key_id=$AWS_ACCESS_KEY_ID \
  -e aws_secret_access_key=$AWS_SECRET_ACCESS_KEY \
  -e gh_token=$GH_TOKEN \
  -e action=cleanup \
  -e guid=$USER-win2025 \
  -e disable_http2=true
```
