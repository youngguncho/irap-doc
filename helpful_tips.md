# Helpful Tips

## fstab 설정(윈도우 공용 파티션 설정)
윈도우랑 우분투에서 공동으로 작업하게 되는 폴더들을 위해서 (svn 내의 reference나 문서들)
부팅시 자동으로 mount되게 하려면 /etc/fstab에 아래의 라인을 추가하면 됩니다.

* /dev/sdb3(해당 파티션)   /mnt/data(마운트위치)   ntfs(파티션 포멧)    umask=022,uid=1000,gid=1000,dmask=027,fmask=137    0   2

위에서 /dev/sdb3 부분과 어디에 mount할지 개인의 취향에 따라 설정하는 부분만 바꾸어주면 됩니다.

## 디버깅 팁

valgrind

Valgrind는 Linux-x86 용 실행 파일의 디버깅과 프로파일링을 위한 오픈 소스 툴이다. Valgrind는 Memcheck이나 Addrcheck 툴을 사용하여 실행중인 프로그램에서 메모리 누출(leak)/오염(corruption)을 찾아낼 수 있다. 그 외의 Cachegrind, Helgrind 툴을 사용하여 캐쉬 프로파일링을 하거나, 멀티 쓰레드에서의 데이타 경쟁을 발견을 할 수 있다. 이 프로그램은 Julian Seward가 개발했다.

```{.no-highlight}
sudo apt-get install valgrind
```

## System Load indicator(시스템 부하감시) startup 설정
![System Load indicator image](https://ci3.googleusercontent.com/proxy/pUkEEUxHAwvWPJ31wjSVcPIwRw7Hs99Tt7l_E6JRJT2xUGsOk1CGr94SeI3cIn27LHIAf-MZG31lYdnUN8bnWbTMSONKAqCwhckA5AWRyH6akBqZGStaOtSWQi55P3IOnosfDHyBxWKshEfQ-dRMHqTp9SI=s0-d-e1-ft#http://www.dailylinuxnews.com/blog/wp-content/uploads/2014/08/ubuntu_indicator_system_load.png)

위 그림과 같이 cpu/memory 등의 상태를 모니터링 하는 위젯을 추가하는 방법입니다. 
우분투 13.04 이후 부터는 추가로 install 해야 사용 가능합니다. 

```{.no-highlight}
sudo apt-add-repository ppa:indicator-multiload/stable-daily
sudo apt-get update
sudo apt-get install indicator-multiload
```

설치 후 실행하면 위젯이 실행되며 preference에서 취향에 맞게 사용하시면 됩니다. 

_추가적으로 덧붙이면_
나중에 gui가 어려운 경우에는 **top** 이라는 명령을 사용하시면 됩니다.
top은 multi thread 의 경우에도 thread 별 track 이 가능합니다.
