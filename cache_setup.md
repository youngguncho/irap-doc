### 캐시 메모리
* 캐시 메모리 관련 링크

[캐시 메모리 링크](http://zetawiki.com/wiki/%EB%A6%AC%EB%88%85%EC%8A%A4_%EB%A9%94%EB%AA%A8%EB%A6%AC_%EC%82%AC%EC%9A%A9%EB%A5%A0_%ED%99%95%EC%9D%B8/)


* 캐시 메모리 사용

Linux는 I/O 성능을 높이기 위해서 Page Cache를 사용한다. 이 글에서는 Page Cache에 대해서는 다루지 않지만 간단히 설명하면 다음과 같다. Linux는 물리적인 저장/통신 장치와 데이터를 주고 받을 때 메모리에 먼저 적재한 후에 데이터를 주고 받는데 이는 동일한 데이터에 대한 접근을 할 경우 메모리에서 바로 가져오도록 하여 I/O 성능을 높이기 위함이다. 이를 Page라는 단위로 관리를 하며 흔히 Page Cache라고 이야기 한다.
따라서, 한번이라도 데이터를 읽거나 쓴 적이 있다면 메모리는 Page Cache에 적재되고 아래의 파일에서 Cached 영역으로 표기 된다.

출처: http://tumblr.lunatine.net/post/28546340998/faq-linux-%EB%A9%94%EB%AA%A8%EB%A6%AC-%ED%9A%A8%EC%9C%A8%EC%9D%84-%EC%9C%84%ED%95%9C-vfscachepressure

## 캐시 메모리 설정

위와 같은 이유로 일정 이상 cache로 잡히지 않도록, 또는 일정 이상으로 free memory를 남기도록 하여 메모리 부족 문제를 해결할 수 있다. 또한 커널단에서 강제로 cache memory drop을 실행하여 cache memory를 비울 수 있다.

## Slab / Dirty ratio 설정

### vfs_cache_pressure

Linux 커널의 vm 구조와 관련된 파라미터로 vfs_cache_pressure라는 것이 존재한다. 이 파라미터는 디렉토리와 inode 오브젝트에 대한 캐시로 사용된 메모리를 반환(reclaim)하는 경향의 정도를 지정하는 항목이다. 기본 값은 100. 
이 값을 0으로 설정하게 되면 Linux 커널은 오브젝트에 대한 캐시를 반환하려고 하지 않을 것이며 얼마 지나지 않아 시스템은 Out of Memory 상태를 호소할 것이다.
그리고, 100 이상의 값을 주면 Linux 커널은 오브젝트에 대한 캐시를 가급적 반환하려고 하며 (다른 말로 가급적 캐시해서 보관하려고 하지 않으려 든다) 이를 이용하면 inode와 dentry 캐시를 줄일 수가 있다.

일시적인 설정
> echo 10000 > /proc/sys/vm/vfs_cache_pressure

영구적인 설정
> add vm.vfs_cache_pressure = 10000 at /etc/sysctl.conf

### min_free_kbytes

시스템 전체에 걸쳐 빈 공간으로 두는 최소 크기 (KB)입니다. 이 값은 각각의 낮은 메모리 영역에서 워터마크 값을 계산하는데 사용되며 그 후 그 크기에 비례하여 저장된 빈 페이지 수를 할당합니다.

min_free_kbytes를 너무 낮게 설정하면 시스템이 메모리 회수를 실행하지 못할 수 있습니다. 이로 인해 시스템이 중단되고 메모리 부족으로 인해 여러 프로세스가 종료될 수 있습니다.
하지만 이러한 매개 변수를 너무 높은 값 (총 시스템 메모리의 5-10%)으로 설정하면 바로 시스템 메모리 부족을 초래하게 됩니다. Linux는 캐시 파일 시스템 데이터에 사용 가능한 모든 RAM을 사용하도록 설계되어 있습니다. min_free_kbytes 값을 높게 설정하면 시스템이 메모리를 회수하는데 너무 많은 시간을 소모하게 됩니다.

일시적인 설정
> echo 70000(원하는 값) > /proc/sys/vm/min_free_kbytes

영구적인 설정
> add vm.min_free_kbytes = 70000(원하는 값) at /etc/sysctl.conf

### dirty_ratio

백분율 값을 정의합니다. 더티 데이터의 Writeout은 (pdflush를 통해) 더티 데이터가 총 시스템 메모리의 이러한 퍼센트를 차지하는 때에 시작됩니다. 기본값은 10입니다.

일시적인 설정
> echo 10(원하는 값) > /proc/sys/vm/dirty_ratio

영구적인 설정
> add vm.dirty_ratio = 10(원하는 값) at /etc/sysctl.conf

### dirty_background_ratio
백분율 값을 정의합니다. 더티 데이터의 Writeout은 (pdflush를 통해) 더티 데이터가 총 시스템 메모리의 이러한 퍼센트를 차지하는 때에 시작됩니다. 기본값은 20입니다.

일시적인 설정
> echo 10(원하는 값) > /proc/sys/vm/dirty_background_ratio

영구적인 설정
> add vm.dirty_background_ratio = 10(원하는 값) at /etc/sysctl.con

## 캐시 메모리 비우기
To free pagecache 
> sudo sync && echo 3 | sudo tee /proc/sys/vm/drop_caches

To free dentries and inodes
> sudo sync && echo 2 | sudo tee /proc/sys/vm/drop_caches

To free pagecache, dentries and inodes
> sudo sync && echo 3 | sudo tee /proc/sys/vm/drop_caches
