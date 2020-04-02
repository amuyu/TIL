
# block 파일 위치
아래 위치에 blcok 파일들이 쌓인다
```sh
# peer
/var/hyperledger/production/ledgersData/chains/chains/{channel-name}
# orderer
/var/hyperledger/production/orderer/chains/{channel-name}
```

# 체인코드 트랜잭션 흐름에 따른 데이터 - proposal.proto 참조
## client -> Endorser
SignedProposal
|\_ Signature
 \_ Proposal
    |\_ Header
    |\_ ChaincodeProposalPayload
     \_ ChaincodeAction

## Endorser -> Client
ProposalResponse
|\_ Endorsement
 \_ ProposalResponsePayload
    |\_ ChaincodeAction

## Client -> Orderer
SignedProposal
|\_ Signature
 \_ Transaction
    |\_ TransactionAction (1..n)
        |\_ Header (1)
         \_ ChaincodeActionPayload (1)
            |\_ ChaincodeProposalPyload (1)
             \_ ChaincodeEndorsedAction (1)
                |\_ Endorsement (1...n)
                 \_ ProposalResponsePayload
                    |\_ ChaincodeACtion


SignedTransaction
|\_ Signature                                    (signature on the Transaction message by the creator specified in the header)
 \_ Transaction
     \_ TransactionAction (1...n)
        |\_ Header (1)                           (the header of the proposal that requested this action)
         \_ ChaincodeActionPayload (1)
            |\_ ChaincodeProposalPayload (1)     (payload of the proposal that requested this action)
             \_ ChaincodeEndorsedAction (1)
                |\_ Endorsement (1...n)          (endorsers' signatures over the whole response payload)
                 \_ ProposalResponsePayload
                     \_ ChaincodeAction          (the actions for this proposal)


# explorer 참고
## block

## transaction obj (SyncService)
txObj = block.data.data
txObj
|\_ payload
    |\_ data
        |\_ actions
            |\_ payload
                |\_ chaincode_proposal_payload.input.chaincode_spec.input.args
                |\_ action
                    |\_ endorsements
                        |\_ endorser.Mspid
                    |\_ proposal_response_payload
                        |\_ extension
                            |\_ chaincode_id.name
                             \_ response.status
                             \_ results_ns_rwset


## transaction 정리
block 조회 후, 얻는 `BlockInfo` 객체에서 block.envelopeInfo.type 이 TRANSACTION_ENVELOPE 인게 transaction
### transactionActionInfo
- responseStatus : 200, 400, 500


### transaction input argmument
0: functions, 1~: args

# ref 
[하이퍼레저 패브릭 데이터 구조](https://www.slideshare.net/logpresso/ss-112815814)