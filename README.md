## Windows Server Failover Cluster 실습 (201910 Windows 교육)

## LAB 18: Failover Cluster 


### - 필요 서버: AD+iSCSI 서버 1EA, Failover Cluster Node 2EA
![1570175418284](https://user-images.githubusercontent.com/20136723/66191171-ae157400-e6c8-11e9-9f6c-c9fc8e780f72.png)


### - 진행 내용

0. **사전 진행사항**

   A. 가상 스위치 생성

   ```powershell
   [Hyper-V 호스트에서 수행]
   - Powershell (관리자로 실행)
   New-VMSwitch -Name Private -SwitchType Internal
   New-VMSwitch -Name Storage -SwitchType Private
   New-VMSwitch -Name Heartbeat -SwitchType Private
   ```

   ```commandline
   - command (관리자로 실행)
   netsh int ip set addr name="vEthernet (Private)" static 192.168.56.1 255.255.255.0
   ```

   B. 가상 머신 가져오기

   ```powershell
   1) USB의 VM 폴더를 C:\에 복사한다
   2) Powershell (관리자로 실행)
   
   Import-VM -Path 'C:\VM\w2012r2ad\Virtual Machines\C266D632-8B60-43A8-9B9D-AE5B91E827EC.vmcx' -Copy -GenerateNewId
   
   Import-VM -Path 'C:\VM\w2012r2fc1\Virtual Machines\1DCB9B69-A114-49CA-A633-3EFE8E28B1D0.vmcx' -Copy -GenerateNewId
   
   Import-VM -Path 'C:\VM\w2012r2fc2\Virtual Machines\630F996A-8127-4FE9-A6F0-412DB370C0D6.vmcx' -Copy -GenerateNewId
   ```

   예상결과
    ```powershell
    PS C:\Windows\system32> Import-VM -Path 'C:\VM\w2012r2ad\Virtual Machines\C266D632-8B60-43A8-9B9D-AE5B91E827EC.vmcx' -Copy -GenerateNewId
    Name      State CPUUsage(%) MemoryAssigned(M) Uptime   Status    Version
    ----      ----- ----------- ----------------- ------   ------    -------
    w2012r2ad Off   0           0                 00:00:00 정상 작동 8.0
    ```
   
   확인사항
   1) (ad, fc1, fc2) 바탕화면 > ping_check 실행
   2) (fc1, fc2) 시작 > 실행 > diskmgmt.msc > disk1, disk2 확인
   

1. **AD+iSCSI 용 서버 OS 설정 (생략)**

   A. AD 설치

   B. iSCSI 설치 (https://dreamlog.tistory.com/565)

   

2. **Failover Cluster 노드 OS 설정 (w2012r2fc1, w2012rcfc2 에서 수행)**

   A. AD 조인 (교재 172 페이지)

   ​	1) DNS 설정

   - 시작 - 실행 - ncpa.cpl - 확인 
   -  Private 선택 우측마우스 - Properties - IPv4 선택 -   Properties 
   -  DNS Server를 **192.168.56.10**으로 설정

   ​	2) 멤버 Join
   
   ※ 주의: 도메인 Join 이후 로그인 계정 → **local.com\administrator**
   

   B. iSCSI initiator 로 iSCSI target 서버에 연결 (생략)

   C. Failover Cluster Role 설치 (fc1, fc2 둘다 수행) (교재 174 페이지)

   D. 장애조치 클러스터 관리자 > 클러스터 만들기 (fc1 에서 수행) (교재 174 페이지)

   - cluster name: testCluster
   - cluster Network: 
     - 172.16.0.0/24, 10.10.10.0/24 체크 제외
     - 192.168.56.0/24 네트워크의 Address에 192.168.50.20 입력
   


