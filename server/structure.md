
# 프로젝트 구조 및 설계

## basic
* interfaces : controller, dto, mapper
* application : facade
* domain : entity, service, command, criteria, info, reader, store, executor, factory...
* infrastructure : jpa, low level

## naming
* xxxReader
* xxxStore
* xxxExecutor
* xxxFactory
* xxxAggregator



domain

// Commnad, Criteria - Info
PartnerServiceImpl
partnerreader
partnerstore

// infrastructure - jpa 연동

