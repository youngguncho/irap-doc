# NTP time sync 설정 (Window server - Ubuntu Client)
## Window server

1. NTP서버 유효화  
  레지스트리 편집기(regedit)를 열고, 두개의 키값을 아래와 같이 수정
	* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer  
	값：Enabled = 1  
	* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Config  
	값：AnnounceFlags = 5  


2. 방화벽 인바운드 설정    
    * 방화벽 > 시스템 및 보안 > Windows방화벽 > 고급설정 에서
    새규칙 추가 -> 포트 선택 -> UDP 및 특정 로컬 포트(값 123) -> 연결허용 -> 다음 -> 이름(사용자 지정) -> 마침   
3. 관리자 권한으로 cmd 실행  
    * net stop w32time && net start w32time

## Ubuntu client
  > sudo apt-get install ntp  
  > sudo service stop ntp  
  > sudo ntpdate -u [Server ip]
    
  

