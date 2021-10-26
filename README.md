## Install Prerequisites
```
pip3 install requests-credssp
pip3 install pywinrm
```

## Configure Windows Host
Download [ConfigureRemotingForAnsible.ps1](https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1)
```
powershell.exe -File ConfigureRemotingForAnsible.ps1 -Verbose -EnableCredSSP
```

## Example hosts.yml
Example hosts file:
```
cat << EOF > hosts.yml
lansweeper:
  hosts:
    lansweeper-app:
EOF
```

## Example vars.yml
Example variables to deploy the CertifyTheWeb ACME client to a Windows server:
```
cat << EOF > vars.yml
match_host: lansweeper
ansible_connection: winrm
ansible_winrm_transport: credssp
ansible_winrm_server_cert_validation: ignore
package_url: http://192.168.1.1/
ansible_user: "username@domain.com"
ansible_password: "password"
package_name: CertifyTheWebSetup_V5.5.6.exe
install_params: /VERYSILENT /SUPPRESSMSGBOXES /NORESTART /SP-
license_email: email@company.org
license_key: 00000000-0000-0000-0000-000000000000
EOF
```

## Deploy Certify The Web
```
ansible-playbook deploy.yml -i hosts.yml -e @vars.yml
```

## License Certify The Web
```
ansible-playbook license.yml -i hosts.yml -e @vars.yml
```