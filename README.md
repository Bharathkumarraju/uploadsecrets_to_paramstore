# uploadsecrets_to_paramstore
how to upload secrets to ssm store

### Install ansible somehow
`brew install ansible (or) pip install ansible`
`And make sure you have correct AWS access credentials to use AWS's SSM Parameter store`

### encrypt the vars/main.yml as below
```
bharath@Bharaths-Mac(master) $ ansible-vault encrypt --vault-password-file .pass_file  vars/main.yml 
Encryption successful
bharath@Bharaths-Mac(master) $
```

### view and edit later your environemnts in vars/main.yml as below
```
bharath@Bharaths-Mac(master) $ ansible-vault view --vault-password-file .pass_file  vars/main.yml 
bharath@Bharaths-Mac(master) $

bharath@Bharaths-Mac(master) $ ansible-vault edit --vault-password-file .pass_file  vars/main.yml 
bharath@Bharaths-Mac(master) $
```

### Run playbook you can run from your local or your CI tool like jenkins or github actions or bamboo etc...
### From CI tool advantage is you can store vault_pass in secrets(currently i have stored in repo itself for demo only)

### use `no_log: true`  not to display passwords in ansible output :)

```
bharath@Bharaths-Mac(master) $ ansible-playbook playbook.yml -vv  --vault-password-file .pass_file
ansible-playbook 2.8.3
  config file = None
  configured module search path = ['/Users/bharathdasaraju/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/local/lib/python3.7/site-packages/ansible
  executable location = /usr/local/bin/ansible-playbook
  python version = 3.7.4 (default, Sep  7 2019, 18:27:02) [Clang 10.0.1 (clang-1001.0.46.4)]
No config file found; using defaults
 [WARNING]: No inventory was parsed, only implicit localhost is available

 [WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'


PLAYBOOK: playbook.yml ******************************************************************************************************************************************************************************
1 plays in playbook.yml

PLAY [localhost] ************************************************************************************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************************************************************************
task path: /Users/bharathdasaraju/external/uploadsecrets_to_paramstore/playbook.yml:2
ok: [localhost]

TASK [Load variables for the particular environment] ************************************************************************************************************************************************
task path: /Users/bharathdasaraju/external/uploadsecrets_to_paramstore/playbook.yml:6
ok: [localhost] => (item=None) => {"censored": "the output has been hidden due to the fact that 'no_log: true' was specified for this result", "changed": false}
ok: [localhost] => {"censored": "the output has been hidden due to the fact that 'no_log: true' was specified for this result", "changed": false}
META: ran handlers

TASK [test simple command] **************************************************************************************************************************************************************************
task path: /Users/bharathdasaraju/external/uploadsecrets_to_paramstore/playbook.yml:12
changed: [localhost] => {"changed": true, "cmd": ["uname", "-a"], "delta": "0:00:00.004834", "end": "2019-10-02 08:31:30.498955", "rc": 0, "start": "2019-10-02 08:31:30.494121", "stderr": "", "stderr_lines": [], "stdout": "Darwin Bharaths-MacBook-Pro.local 18.5.0 Darwin Kernel Version 18.5.0: Mon Mar 11 20:40:32 PDT 2019; root:xnu-4903.251.3~3/RELEASE_X86_64 x86_64", "stdout_lines": ["Darwin Bharaths-MacBook-Pro.local 18.5.0 Darwin Kernel Version 18.5.0: Mon Mar 11 20:40:32 PDT 2019; root:xnu-4903.251.3~3/RELEASE_X86_64 x86_64"]}

TASK [Always update a parameter store value and create a new version] *******************************************************************************************************************************
task path: /Users/bharathdasaraju/external/uploadsecrets_to_paramstore/playbook.yml:15
changed: [localhost] => (item=None) => {"censored": "the output has been hidden due to the fact that 'no_log: true' was specified for this result", "changed": true}
changed: [localhost] => (item=None) => {"censored": "the output has been hidden due to the fact that 'no_log: true' was specified for this result", "changed": true}
changed: [localhost] => (item=None) => {"censored": "the output has been hidden due to the fact that 'no_log: true' was specified for this result", "changed": true}
changed: [localhost] => (item=None) => {"censored": "the output has been hidden due to the fact that 'no_log: true' was specified for this result", "changed": true}
changed: [localhost] => (item=None) => {"censored": "the output has been hidden due to the fact that 'no_log: true' was specified for this result", "changed": true}
changed: [localhost] => {"censored": "the output has been hidden due to the fact that 'no_log: true' was specified for this result", "changed": true}
META: ran handlers
META: ran handlers

PLAY RECAP ******************************************************************************************************************************************************************************************
localhost                  : ok=4    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

bharath@Bharaths-Mac(master) $ 

```

### how to use the ansible vault password as environment variable as well check below gist
https://gist.github.com/shawnsi/b13f6a740bddc670e633 