## Windows Server Failover Cluster 실습 (201910 Windows 교육)

## 사전 진행사항

A. 가상 스위치 생성

```powershell
[Hyper-V 호스트에서 수행]
- Powershell (관리자로 실행)
New-VMSwitch -Name Private -SwitchType Internal
New-VMSwitch -Name Storage -SwitchType Private
New-VMSwitch -Name Heartbeat -SwitchType Private
```



B. 가상 머신 가져오기

```powershell
1) USB의 VM 폴더를 C:\에 복사한다
2) Powershell (관리자로 실행)

Import-VM -Path 'C:\VM\w2012r2ad\Virtual Machines\C266D632-8B60-43A8-9B9D-AE5B91E827EC.vmcx' -Copy -GenerateNewId

Import-VM -Path 'C:\VM\w2012r2fc1\Virtual Machines\1DCB9B69-A114-49CA-A633-3EFE8E28B1D0.vmcx' -Copy -GenerateNewId

Import-VM -Path 'C:\Users\SDS\Desktop\VM\w2012r2fc1\Virtual Machines\1DCB9B69-A114-49CA-A633-3EFE8E28B1D0.vmcx' -Copy -GenerateNewId
```

