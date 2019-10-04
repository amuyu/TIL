
Endorsement policies work at two different granularities: 
they can be set for an entire namespace, as well as for individual state keys. 
They are formulated using basic logic expressions such as `AND` and `OR`.

# syntax
'Org0.admin': any administrator of the Org0 MSP
'Org1.member': any member of the Org1 MSP
'Org1.client': any client of the Org1 MSP
'Org1.peer': any peer of the Org1 MSP

# ref
[endorsement](https://hyperledger-fabric.readthedocs.io/en/release-1.4/developapps/endorsementpolicies.html?highlight=endorsement)
[two-ways-to-require-endorsement](https://hyperledger-fabric.readthedocs.io/en/release-1.4/endorsement-policies.html?highlight=endorsement#two-ways-to-require-endorsement)