# Issue1


`Hey Guys,
I am getting following error while trying to run a playbook. I wonder why ssh is failing on the same machine. I tried to change it to localhost but still same error. Any clue ?
`TASK [Gathering Facts]`
*************************************************************************************************************************************************
Wednesday 25 September 2019  07:29:33 -0400 (0:00:00.062)       0:00:01.104 ***
fatal: [rqa-os-master.rtp.raleigh.ibm.com]: UNREACHABLE! => {"changed": false, "msg": "SSH Error: data could not be sent to remote host \"rqa-os-master.rtp.raleigh.ibm.com\". Make sure this host can be reached over ssh", "unreachable": true}

**Resolution**

Take a look if your sshd_config is using Match..... you may have PubkeyAuthentication set to no 
for your ansible user. Make sure PubkeyAuthentication is set to yes for it. You can add your ansible user to the whitelst group
