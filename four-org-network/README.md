#### To Access Tutorial Provided by IBM Blockchain Platform
click `IBM Blockchain Platform extension` -> `Add Environment` (in FABRIC ENVIRONMENTS) -> `Create new from template` (from pop-up) -> `Create additional local network (tutorial)`

# Installation 
### Pre-requisites
- node v14.15.0
- Python 3.7+ - [Official Installation Document](https://www.python.org/downloads/)
- Pip - [Official Installation Document](https://pip.pypa.io/en/stable/installing/)
- Ansible - [Official Installation Document](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- Hyperledger Fabric v1.4 binaries - [Official Installation Document](https://hyperledger-fabric.readthedocs.io/en/release-1.4/install.html)
- Docker 19.03+ - [Official Installation Document](https://docs.docker.com/engine/install/ubuntu/#installation-methods)
- jq - [Official Installation Document](https://stedolan.github.io/jq/download/)
- sponge 
- IBM bloackchain platform manager ansible role  

#### Unbuntu
```bash
# install python3
sudo apt update
sudo apt install python3

# install pip
sudo apt install python3-pip

# install ansible
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install ansible

# install fabric example, make sure you know where it is being downloaded, you will need it in the Note section
curl -sSL http://bit.ly/2ysbOFE | bash -s

# install docker
sudo apt-get install docker-ce docker-ce-cli containerd.io

# install docker SDK for python
pip3 install docker

# install jq
sudo apt-get install jq

# install sponge
sudo apt-get install moreutils 

# install ansible role
ansible-galaxy install ibm.blockchain_platform_manager
```

> Note: 
> After installing Hyperledger Fabric binaries, make sure you add the path in the `.bashrc`, `.zshrc` or other shell script your terminal uses. Add the below line to the end of the file
> ```bash
> export PATH=${PATH}:/[download path for hyperledger fabric]/fabric-samples/bin
> ```
> After saving and closing the shell script, make sure you run `source .bashrc` (or `source .zshrc`, etc) in the terminal where you run `ansible`.


#### MacOS
```bash
brew install python3
pip3 install ansible
curl -sSL http://bit.ly/2ysbOFE | bash -s 
brew install jq
brew install moreutils
ansible-galaxy install ibm.blockchain_platform_manager
```

### setup
```bash
git clone git@github.com:Mavennet/blockchain-network-ansible.git
cd blockchain-network-ansible/four-org-network
```

# To Run
Run this command in `blockchain-network-ansibl/four-org-network` folder
```bash
ansible-playbook playbook.yml 
```
Once the network is setup succesfully, you should see something similiar to the following when you do `docker ps`
```bash
CONTAINER ID   IMAGE                                                                                                            COMMAND                  CREATED        STATUS        PORTS                                            NAMES
df7cc13be2c6   dev-org3peer2-neoflow-oil-traceability-0.0.30-ac10a585fdfacf23a5be67558071cb2a081a5d4175ecaedee1b506012db7637d   "/bin/sh -c 'cd /usr…"   24 hours ago   Up 24 hours                                                    dev-Org3Peer2-neoflow-oil-traceability-0.0.30
1e268b5862f4   dev-org4peer2-neoflow-oil-traceability-0.0.30-51f72c62659784b77118454fca103a7af489ab1b66c6bab377aa59ce3e8fe565   "/bin/sh -c 'cd /usr…"   24 hours ago   Up 24 hours                                                    dev-Org4Peer2-neoflow-oil-traceability-0.0.30
7790d0405dde   dev-org4peer1-neoflow-oil-traceability-0.0.30-57457205ab462f280f6ed2defce76d3806abdc02eac377e34638b8f66cac01b5   "/bin/sh -c 'cd /usr…"   24 hours ago   Up 24 hours                                                    dev-Org4Peer1-neoflow-oil-traceability-0.0.30
c73c17240c15   dev-org3peer1-neoflow-oil-traceability-0.0.30-624bd6fa3aded24a1064e9823eca49c91cb42cef270131b06eb5920401b212e6   "/bin/sh -c 'cd /usr…"   25 hours ago   Up 25 hours                                                    dev-Org3Peer1-neoflow-oil-traceability-0.0.30
68a206bdb226   dev-org2peer1-neoflow-oil-traceability-0.0.30-5366bb5f064cd85573a4f0d7719e53f50f7159062cabd14ee3709fe7e44c5c2e   "/bin/sh -c 'cd /usr…"   2 days ago     Up 2 days                                                      dev-Org2Peer1-neoflow-oil-traceability-0.0.30
1345277fe623   dev-org2peer2-neoflow-oil-traceability-0.0.30-978e1de0ac37b9b6f21806db3892c05998090fd3f931f9a4c07d72b22d530a2f   "/bin/sh -c 'cd /usr…"   2 days ago     Up 2 days                                                      dev-Org2Peer2-neoflow-oil-traceability-0.0.30
3a6996b1f521   dev-org1peer2-neoflow-oil-traceability-0.0.30-7c3d4794ea795d0fc330580f750162a6c7c21a7638a72847d9181ff0d3cf5e74   "/bin/sh -c 'cd /usr…"   2 days ago     Up 2 days                                                      dev-Org1Peer2-neoflow-oil-traceability-0.0.30
3b280319e24c   dev-org1peer1-neoflow-oil-traceability-0.0.30-d5a6bf124e6f46024aa29286e703bcd88eec5730d6534fac48291fc8a2aa9bb1   "/bin/sh -c 'cd /usr…"   2 days ago     Up 2 days                                                      dev-Org1Peer1-neoflow-oil-traceability-0.0.30
272ec861cec1   hyperledger/fabric-orderer:1.4.6                                                                                 "orderer"                6 days ago     Up 2 days     7050/tcp, 0.0.0.0:17051-17052->17051-17052/tcp   orderer.example.com
0c92e6b0cee2   hyperledger/fabric-ca:1.4.6                                                                                      "sh -c 'fabric-ca-se…"   6 days ago     Up 2 days     7054/tcp, 0.0.0.0:17050->17050/tcp               ca.orderer.example.com
1c78b0b2c353   hyperledger/fabric-peer:1.4.6                                                                                    "peer node start"        6 days ago     Up 2 days     0.0.0.0:21055-21057->21055-21057/tcp             peer1.org4.example.com
b3139837dc90   hyperledger/fabric-peer:1.4.6                                                                                    "peer node start"        6 days ago     Up 2 days     0.0.0.0:21051-21053->21051-21053/tcp             peer0.org4.example.com
7c3c655c2d19   hyperledger/fabric-ca:1.4.6                                                                                      "sh -c 'fabric-ca-se…"   6 days ago     Up 2 days     7054/tcp, 0.0.0.0:21050->21050/tcp               ca.org4.example.com
7af52b30b641   hyperledger/fabric-peer:1.4.6                                                                                    "peer node start"        6 days ago     Up 2 days     0.0.0.0:20055-20057->20055-20057/tcp             peer1.org3.example.com
b0772b2b8a08   hyperledger/fabric-peer:1.4.6                                                                                    "peer node start"        6 days ago     Up 2 days     0.0.0.0:20051-20053->20051-20053/tcp             peer0.org3.example.com
6dda01a355d5   hyperledger/fabric-ca:1.4.6                                                                                      "sh -c 'fabric-ca-se…"   6 days ago     Up 2 days     7054/tcp, 0.0.0.0:20050->20050/tcp               ca.org3.example.com
d02139113c98   hyperledger/fabric-peer:1.4.6                                                                                    "peer node start"        6 days ago     Up 2 days     0.0.0.0:19055-19057->19055-19057/tcp             peer1.org2.example.com
f97e519571d4   hyperledger/fabric-peer:1.4.6                                                                                    "peer node start"        6 days ago     Up 2 days     0.0.0.0:19051-19053->19051-19053/tcp             peer0.org2.example.com
90b166865e2c   hyperledger/fabric-ca:1.4.6                                                                                      "sh -c 'fabric-ca-se…"   6 days ago     Up 2 days     7054/tcp, 0.0.0.0:19050->19050/tcp               ca.org2.example.com
5e0af32c24e1   hyperledger/fabric-peer:1.4.6                                                                                    "peer node start"        6 days ago     Up 2 days     0.0.0.0:18055-18057->18055-18057/tcp             peer1.org1.example.com
fac658770843   couchdb:2.3.1                                                                                                    "tini -- /docker-ent…"   6 days ago     Up 2 days     4369/tcp, 9100/tcp, 0.0.0.0:18058->5984/tcp      couchdb1.org1.example.com
1c1acf0c76f7   hyperledger/fabric-peer:1.4.6                                                                                    "peer node start"        6 days ago     Up 2 days     0.0.0.0:18051-18053->18051-18053/tcp             peer0.org1.example.com
7f7fa8c30ed9   couchdb:2.3.1                                                                                                    "tini -- /docker-ent…"   6 days ago     Up 2 days     4369/tcp, 9100/tcp, 0.0.0.0:18054->5984/tcp      couchdb0.org1.example.com
1b2caa3802b8   hyperledger/fabric-ca:1.4.6                                                                                      "sh -c 'fabric-ca-se…"   6 days ago     Up 2 days     7054/tcp, 0.0.0.0:18050->18050/tcp               ca.org1.example.com

```

# To Clean up
Run this command in `blockchain-network-ansibl/four-org-network` folder
```bash
ansible-playbook uninstall.yml # this command should be ran in /four-org-network folder
```

# Importing to Fabric Network in VScode IBM Blockchain Platform
Once your Ansible playbook has successfully ran, `nodes`, `gateways` and `wallets` directories should have been created.

These contain all of the files needed to import your network and start interacting with it!

1. Hover over the `Fabric Environments` panel on the left hand side of the screen and select the +.
2. You will then be asked to `Select a method to add an environment`. Choose the `Add an Ansible-created network` option.
3. You will then be prompted to select a directory to import.
> If you have ran the the `four-org-network` playbook, you should select this directory.
4. Give your network a name e.g. `ansibleNetwork`. Whatever you find easy to identify! Then press `Enter` which will import your Ansible network.
5. If your Ansible Fabric network is running, you should see your new environment imported and any gateways and wallets.
> Note: If you don't see any gateways and wallets, it could be because your Fabric network hasn't been started.
>
> Once you have run your Ansible playbook and your network has started, you can refresh the `Fabric Gateways` and `Fabric Wallets` panels to see your gateways and wallets.

6. If your `four-org-network` generated Ansible Fabric network is running, you will see gateways `ansibleNetwork > Org[1-4] gateway` and wallets `ansibleNetwork > Org[1-4]`.
>

# Resolve Errors

### fabric-ca-client Error (or other fabric related errors):
```
"cmd": "fabric-ca-client enroll -u 'https://admin:********@localhost:18050' --caname ca -M /[Directory Path]/blockchain-network-ansible/four-org-network/wallets/Org1/admin --tls.certfiles /[Directory Path]/blockchain-network-ansible/four-org-network/wallets/Org1/ca-tls-root.pem", 
"msg": "[Errno 2] No such file or directory: b'fabric-ca-client'", 
"rc": 2
```
> solution:
> install Hyperledger Fabric v1.4 binaries `curl -sSL http://bit.ly/2ysbOFE | bash -s `


### Signature Error (or other resource update related errors):
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
> solution:
> `ansible-playbook uninstall.yml`



### Invalid Chunk Length Error:

```
TASK [ibm.blockchain_platform_manager : Start peer container] ************************************************************************************************************************************************
fatal: [localhost]: FAILED! => {
  "changed": false, 
  "msg": "Error inspecting container: (\"Connection broken: InvalidChunkLength(got length '', 0 bytes read)\", InvalidChunkLength(got length '', 0 bytes read))"
}
```
> solution: 
> clean all docker container and images



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
> solution:
> check in the yml file, make sure `endorsement_policy` does not contain spaces in the string. 


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
