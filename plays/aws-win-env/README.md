# aws-win-env

This playbook is intended to provide an easily deployable Windows instance in AWS for testing DLS in an environment that mimics enterprise environment limitations such as:
- Non-Admin user
- HTTP/2 disabled
- WDAC Profiles and AppLocker (TBD)

## Prerequisites
If on Fedora:
```bash
sudo dnf -y install ansible python3-winrm python3-boto3
```

On other platforms:
```bash
pip install ansible pywinrm
ansible-galaxy collection install amazon.aws
```

## Usage:
```bash
ansible-playbook ./ansible-windows-extension.yml \
  -e aws_access_key_id=$AWS_ACCESS_KEY_ID \
  -e aws_secret_access_key=$AWS_SECRET_ACCESS_KEY \
  -e gh_token=$GH_TOKEN \
  -e user_email=$EMAIL_ADDRESS \
  -e action=deploy \
  -e guid=$USER-win2025 \
  -e disable_http2=true
```

By default the latest Konveyor AI release, excluding pre-releases will be installed. If you need an alternate version provide a link to the vsix
```bash
ansible-playbook ./ansible-windows-extension.yml \
  -e aws_access_key_id=$AWS_ACCESS_KEY_ID \
  -e aws_secret_access_key=$AWS_SECRET_ACCESS_KEY \
  -e gh_token=$GH_TOKEN \
  -e user_email=$EMAIL_ADDRESS \
  -e action=deploy \
  -e guid=$USER-win2025 \
  -e disable_http2=true \
  -e https://github.com/migtools/editor-extensions/releases/download/development-builds/mta-vscode-extension-8.0.0-dev.20251124223200.c332c41.vsix
```

## Accessing Proxy Logs
The proxy is run as a service on the proxy host. In order to follow the logs connect to the proxy host
```bash
ssh -i $guid-keypair.pem ec2-user@<ip address>
```

And follow the logs
```bash
sudo journalctl -b 0 -u mitmproxy-wireguard -f
```

## To clean up
```bash
ansible-playbook ./ansible-windows-extension.yml \
  -e aws_access_key_id=$AWS_ACCESS_KEY_ID \
  -e aws_secret_access_key=$AWS_SECRET_ACCESS_KEY \
  -e gh_token=$GH_TOKEN \
  -e user_email=$EMAIL_ADDRESS \
  -e action=cleanup \
  -e guid=$USER-win2025 \
  -e disable_http2=true
```
