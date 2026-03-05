# The avilable record types in auditrs include the following:

## Control (1000–1019)

- GetStatus
- SetStatus
- List
- Add
- Del
- User
- Login
- WatchInsert
- WatchRemove
- WatchList
- SignalInfo
- AddRule
- DelRule
- ListRules
- Trim
- MakeEquiv
- TtyGet
- TtySet
- SetFeature
- GetFeature

## User (1100–1199)

- FirstUserMsg
- UserAcct
- UserMgmt
- CredAcq
- CredDisp
- UserStart
- UserEnd
- UserAvc
- UserChauthtok
- UserErr
- CredRefr
- UsysConfig
- UserLogin
- UserLogout
- AddUser
- DelUser
- AddGroup
- DelGroup
- DacCheck
- ChgrpId
- Test
- TrustedApp
- UserSelinuxErr
- UserCmd
- UserTty
- ChuserId
- GrpAuth
- SystemBoot
- SystemShutdown
- SystemRunlevel
- ServiceStart
- ServiceStop
- GrpMgmt
- GrpChauthtok
- MacCheck
- AcctLock
- AcctUnlock
- UserDevice
- SoftwareUpdate
- LastUserMsg

## Daemon (1200–1209)

- DaemonStart
- DaemonEnd
- DaemonAbort
- DaemonConfig
- DaemonReconfig
- DaemonRotate
- DaemonResume
- DaemonAccept
- DaemonClose
- DaemonErr

## Kernel (1300–1339)

- Syscall
- Path
- Ipc
- Socketcall
- ConfigChange
- Sockaddr
- Cwd
- Execve
- IpcSetPerm
- MqOpen
- MqSendRecv
- MqNotify
- MqGetSetAttr
- KernelOther
- FdPair
- ObjPid
- Tty
- Eoe
- BprmFcaps
- Capset
- Mmap
- NetfilterPkt
- NetfilterCfg
- Seccomp
- Proctitle
- FeatureChange
- Replace
- KernModule
- Fanotify
- TimeInjOffset
- TimeAdjNtpVal
- Bpf
- EventListener
- UringOp
- Openat2
- DmCtrl
- DmEvent

## SELinux (1400–1426)

- Avc
- SelinuxErr
- AvcPath
- MacPolicyLoad
- MacStatus
- MacConfigChange
- MacUnlblAllow
- MacCipsoV4Add
- MacCipsoV4Del
- MacMapAdd
- MacMapDel
- MacIpsecAddSa
- MacIpsecDelSa
- MacIpsecAddSpd
- MacIpsecDelSpd
- MacIpsecEvent
- MacUnlblStcAdd
- MacUnlblStcDel
- MacCalipsoAdd
- MacCalipsoDel
- IpeAccess
- IpeConfigChange
- IpePolicyLoad
- LandlockAccess
- LandlockDomain
- MacTaskContexts
- MacObjContexts

## AppArmor (1500–1507)

- Aa
- ApparmorAudit
- ApparmorAllowed
- ApparmorDenied
- ApparmorHint
- ApparmorStatus
- ApparmorError
- ApparmorKill

## Kernel Anomaly (1700–1703)

- AnomalyPromiscuous
- AnomalyAbend
- AnomalyLink
- AnomalyCreat

## Integrity (1800–1808)

- IntegrityData
- IntegrityMetadata
- IntegrityStatus
- IntegrityHash
- IntegrityPcr
- IntegrityRule
- IntegrityEvmXattr
- IntegrityPolicyRule
- IntegrityUserspace

## Legacy (2000)

- Kernel

## User Anomaly (2100–2121)

- AnomalyLoginFailures
- AnomalyLoginTime
- AnomalyLoginSessions
- AnomalyLoginAcct
- AnomalyLoginLocation
- AnomalyMaxDac
- AnomalyMaxMac
- AnomalyAmtuFail
- AnomalyRbacFail
- AnomalyRbacIntegrityFail
- AnomalyCryptoFail
- AnomalyAccessFs
- AnomalyExec
- AnomalyMkExec
- AnomalyAddAcct
- AnomalyDelAcct
- AnomalyModAcct
- AnomalyRootTrans
- AnomalyLoginService
- AnomalyLoginRoot
- AnomalyOriginFailures
- AnomalySession

## Anomaly Response (2200–2215)

- RespAnomaly
- RespAlert
- RespKillProc
- RespTermAccess
- RespAcctRemote
- RespAcctLockTimed
- RespAcctUnlockTimed
- RespAcctLock
- RespTermLock
- RespSebool
- RespExec
- RespSingle
- RespHalt
- RespOriginBlock
- RespOriginBlockTimed
- RespOriginUnblockTimed

## User LSPP (2300–2313)

- UserRoleChange
- RoleAssign
- RoleRemove
- LabelOverride
- LabelLevelChange
- UserLabeledExport
- UserUnlabeledExport
- DevAlloc
- DevDealloc
- FsRelabel
- UserMacPolicyLoad
- RoleModify
- UserMacConfigChange
- UserMacStatus

## User Crypto (2400–2409)

- CryptoTestUser
- CryptoParamChangeUser
- CryptoLogin
- CryptoLogout
- CryptoKeyUser
- CryptoFailureUser
- CryptoReplayUser
- CryptoSession
- CryptoIkeSa
- CryptoIpsecSa

## Virtualization (2500–2507)

- VirtControl
- VirtResource
- VirtMachineId
- VirtIntegrityCheck
- VirtCreate
- VirtDestroy
- VirtMigrateIn
- VirtMigrateOut
