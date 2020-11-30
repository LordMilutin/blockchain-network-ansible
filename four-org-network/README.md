# Pre-requisites
install python 3.7+
install pip
pip install ansible
curl -sSL http://bit.ly/2ysbOFE | bash -s 
brew install jq
brew install moreutils
ansible-galaxy install ibm.blockchain_platform_manager

# To Run
```ansible-playbook playbook.yml```

# To Clean up
```ansible-playbook uninstall.yml```

# Resolve Errors

### Invalid Chunk Length Error:

```
TASK [ibm.blockchain_platform_manager : Start peer container] ************************************************************************************************************************************************
fatal: [localhost]: FAILED! => {
  "changed": false, 
  "msg": "Error inspecting container: (\"Connection broken: InvalidChunkLength(got length '', 0 bytes read)\", InvalidChunkLength(got length '', 0 bytes read))"
}
```
solution: 
clean all docker container and images



### Contract Endorsement Error:
```
TASK [ibm.blockchain_platform_manager : Submit system channel configuration update envelope] ***************************
fatal: [localhost]: FAILED! => {
  "changed": true, 
  "cmd": ["peer", "channel", "update", "-f", "/var/folders/_c/1n9j573122d91nqp0dg1lyxc0000gn/T/ansible.nh9JPk/config_update_as_envelope.pb", "-o", "localhost:17051", "-c", "testchainid", "--tls", "--cafile", "/[Directory Path]/blockchain-network-ansible/four-org-network/wallets/Orderer/tls-root.pem", "--ordererTLSHostnameOverride", "orderer.example.com"], 
  "delta": "0:00:00.041882", 
  "end": "2020-11-27 13:00:08.366540", 
  "msg": "non-zero return code", 
  "rc": 1, 
  "start": "2020-11-27 13:00:08.324658", 
  "stderr": "\u001b[34m2020-11-27 13:00:08.355 EST [channelCmd] InitCmdFactory -> INFO 001\u001b[0m Endorser and orderer connections initialized\nError: got unexpected status: BAD_REQUEST -- error applying config update to existing channel 'testchainid': error authorizing update: error validating DeltaSet: policy for [Value]  /Channel/Consortiums/SampleConsortium/Org1MSP/MSP not satisfied: signature set did not satisfy policy", 
  "stderr_lines": [
    "\u001b[34m2020-11-27 13:00:08.355 EST [channelCmd] InitCmdFactory -> INFO 001\u001b[0m Endorser and orderer connections initialized", 
        "Error: got unexpected status: BAD_REQUEST -- error applying config update to existing channel 'testchainid': error authorizing update: error validating DeltaSet: policy for [Value]  /Channel/Consortiums/SampleConsortium/Org1MSP/MSP not satisfied: signature set did not satisfy policy"
  ], 
  "stdout": "", 
  "stdout_lines": []
}
```
solution:
check in the yml file, make sure `endorsement_policy` does not contain spaces in the string. 



### Signature Error:
```
TASK [ibm.blockchain_platform_manager : Submit system channel configuration update envelope] *****************************************************************************************************************
fatal: [localhost]: FAILED! => {
  "changed": true, 
  "cmd": ["peer", "channel", "update", "-f", "/var/folders/_c/1n9j573122d91nqp0dg1lyxc0000gn/T/ansible.OkzxsI/config_update_as_envelope.pb", "-o", "localhost:17051", "-c", "testchainid", "--tls", "--cafile", "/[Directory Path]/blockchain-network-ansible/one-org-network/wallets/Orderer/tls-root.pem", "--ordererTLSHostnameOverride", "orderer.example.com"], 
  "delta": "0:00:00.034809", 
  "end": "2020-11-30 09:13:38.770120", 
  "msg": "non-zero return code", 
  "rc": 1, 
  "start": "2020-11-30 09:13:38.735311", 
  "stderr": "\u001b[34m2020-11-30 09:13:38.763 EST [channelCmd] InitCmdFactory -> INFO 001\u001b[0m Endorser and orderer connections initialized\nError: got unexpected status: BAD_REQUEST -- error applying config update to existing channel 'testchainid': error authorizing update: error validating DeltaSet: policy for [Value]  /Channel/Consortiums/SampleConsortium/Org1MSP/MSP not satisfied: signature set did not satisfy policy", 
  "stderr_lines": [
    "\u001b[34m2020-11-30 09:13:38.763 EST [channelCmd] InitCmdFactory -> INFO 001\u001b[0m Endorser and orderer connections initialized", 
    "Error: got unexpected status: BAD_REQUEST -- error applying config update to existing channel 'testchainid': error authorizing update: error validating DeltaSet: policy for [Value]  /Channel/Consortiums/SampleConsortium/Org1MSP/MSP not satisfied: signature set did not satisfy policy"
  ], 
  "stdout": "", 
  "stdout_lines": []
}
```
solution:
`ansible-playbook uninstall.yml`







# four-org-network

An Ansible playbook for building a Hyperledger Fabric network with four organizations, `Org1`, `Org2`, `Org3` and `Org4`. Each organization has two peers. The two peers are configured with a single channel, `channel1`. The FabCar sample contract is instantiated on this channel, with an endorsement policy stating that all organizations must endorse any transactions.

To run this Ansible playbook, follow these steps:

1. Ensure that you have all of the pre-requisites installed: https://github.com/IBM-Blockchain/ansible-role-blockchain-platform-manager#requirements

2. Install all required roles, including the `ibm.blockchain_platform_manager` role, from Ansible Galaxy:

    `ansible-galaxy install -r requirements.yml --force`

3. This Ansible playbook defaults to deploying to Docker. To configure the Ansible playbook to deploy to the IBM Blockchain Platform on IBM Cloud, follow these steps:

    1. Edit the playbook such that the `infrastructure.type` variable is set to `saas`:

        ```yaml
        infrastructure:
          type: saas
          saas: "{{ lookup('file', 'service-creds.json') | from_json }}"
        ```

    2. Create a file named `service-creds.json` that contains valid service credentials for an IBM Blockchain Platform service instance on IBM Cloud. These service credentials should be of the format:

        ```json
        {
          "api_endpoint": "https://xxxxxx-optools.uss02.blockchain.cloud.ibm.com",
          "apikey": "xxxxxx",
          "configtxlator": "https://xxxxxx-configtxlator.uss02.blockchain.cloud.ibm.com",
          "iam_apikey_description": "Auto-generated for key xxxxxx",
          "iam_apikey_name": "xxxxxx",
          "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
          "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/xxxxxx::serviceid:ServiceId-xxxxxx"
        }
        ```

4. Run the Ansible playbook:

    `ansible-playbook playbook.yml`

5. Information on the available nodes (peers, orderers, and certificate authorities) will be created under the `nodes` subdirectory.

    1. If you wish to use this network for development purposes, you can import these JSON files into a Fabric Environment using the IBM Blockchain Platform extension for Visual Studio Code.

        For more information on this task, follow the process to create a new Fabric Environment documented here: https://github.com/IBM-Blockchain/blockchain-vscode-extension#connecting-to-another-instance-of-hyperledger-fabric

    2. If you are using the IBM Blockchain Platform on IBM Cloud, you do not need to import these JSON files. All of the nodes will already be present in your web console.

6. The `wallets` subdirectory will contain all of the identities (certificates and private keys) enrolled by this playbook. You must be careful to persist all of these files for the next time you run this playbook, otherwise you will be unable to administer your IBM Blockchain Platform network.

    1. If you wish to use this network for development purposes, you can import these JSON files into a wallet using the IBM Blockchain Platform extension for Visual Studio Code.

        For more information on this task, follow the process to create a new Fabric Environment documented here: https://github.com/IBM-Blockchain/blockchain-vscode-extension#connecting-to-another-instance-of-hyperledger-fabric

    2. If you are using the IBM Blockchain Platform on IBM Cloud, you do need to import these JSON files into your wallet using the web console. You will then need to assiociate each node with the correct identity. If you do not do this, then you will be unable to administer the nodes using the web console.

License
-------

Apache-2.0