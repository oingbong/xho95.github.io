이 자료는 Tiny Core Linux 의 매뉴얼인 **Into the Core** 를 번역 정리한 것입니다. 

### 1 장. 중심 개념 (Core concepts)

#### 1.1 철학 (Philosophies)

짧게 말해서 Tiny Core 는 스스로를 RAM 에 올리고 응용 프로그램을 RAM 에 마운트하거나 설치합니다.

Tiny Core 는 좀 달라서 사용자가 전통 방식인 하드 드라이브에 운영 체제를 설치하는 것을 권장하지 않습니다. 하드 드라이브에 설치하는 것도 물론 가능하지만 Tiny Core 는 부트 시간에 RAM 에 복사되서 실행되도록 설계되었습니다. 

이런 개념이 많은 라이브 CD 들과 유사하게 느껴진다면 기술 자체는 정말로 비슷하다고 할 수 있습니다. 

#### 1.2 간소한 설치 (Frugal insall)

간소한 방식에서는 기본 시스템이 **vmlinuz** 와 **core.gz** 두 개의 파일로 되어 있습니다. 이들의 위치는 부트 로더에 의해 지정됩니다. 

다른 사용자 파일이나 확장들은 기본 OS 의 외부에 저장됩니다. 

#### 1.3 부트 코드 (Boot codes)

Tiny Core 가 설치되는 방식 (GRUB, LILO, CD, USB, ...) 에 따라서 사용자는 매 부트 타임 (CD, ...) 에 부트 코드를 사용하거나 부트 설정 파일 (GRUB, LILO, ...) 에 저장하도록 선택할 수 있습니다. 

부트 코드 (인자) 는 부트 시간에 정의된 옵션에 따라 Tiny Core 가 어떻게 동작할지 영향을 줍니다. 아주 많은 부트 코드가 있습니다. 모든 가능한 옵션을 보려면 CD 부트 프롬프터에서 F2, F3, 또는 F4 를 눌러서 부트 코드 목록을 찬찬히 보기 바랍니다.

부트 코드 중에서 **base** 는 주목할만합니다. **base** 를 사용하면 기본 모드를 시뮬레이트할 수 있고 모든 응용 프로그램 확장의 설치나 마운트를 건너뛸 수 있습니다. 이 기능은 문제 해결, 확장 빌드, 업그레이드... 그리고 Tiny Core 가 대상 하드웨어에서 얼마나 빨리 부팅할 수 있는지 검사할 때 유용합니다.

#### 1.4 USB 와 다른 외부 저장 장치 (USB and other external storage devices)

Tiny Core 는 부팅할 때 USB 펜 드라이브, 컴팩트 플래시 또는 다른 휴대용 미디어 등과 같은 외부 장치의 데이터를 검색하도록 지시할 수 있습니다. 

만약 데이터가 외부 / 저속의 미디어에 저장된 경우, **waitusb=5** 또는 그와 비슷한 부트 코드를 사용해야 할 수 있습니다. 이렇게 하면 부트 과정을 5초 동안 중지시켜서 느린 장치가 시스템 버스에 등록되도록 기다릴 수 있습니다.

#### 1.5 의존 검사 및 내려받기 (Dependency checking and downloading)

Tiny Core 는 가능한 한 쉽게 응용 프로그램을 구할 수 있도록 합니다. 앱 도구 (Apps tool) 는 각각의 **.info** 파일로부터 응용 프로그램의 세부 정보를 제공합니다. 

#### 1.6 작동 모드 (Modes of operation)

작동 모드는 Tiny Core 가 부트 시간에 어떻게 로드, 마운트, 그리고 설치되는지를 버무려 놓습니다. (세 가지 동작의 의미를 구분하는 의미를 알고 싶으면 위의 철학 부분을 보기 바랍니다.) Tiny Core 는 세 가지 주요 모드가 있습니다:

* 기본 (Default) 모드 : cloud / internet
* 마운트 (Mount) 모드 : TCZ / install
* 복사 (Copy) 모드 : TCZ / install + copy2fs / lt

"전통 모드 : 하드 드라이브에 설치" 가 있지 않냐고 할 수도 있겠지만 그건 여기서는 전혀 고려할 대상이 아닙니다. 

#### 1.7 기본 모드 : cloud / internet

기본으로 Tiny Core Linux 는 클라우드 / 인터넷 클라이언트로 동작합니다. 기본 모드는 다음과 같습니다:

Tiny Core 전체가 RAM 에서 부팅합니다. 사용자는 앱 도구 (Apps tool) 로 저장소를 둘러보고 응용 프로그램을 내려 받습니다. 응용 프로그램 확장 (내려받은 응용 프로그램) 은 현재 세션 동안만 지속합니다. Tiny Core 는 가능한 만큼의 RAM 만을 사용합니다.

#### 1.8 마운트 모드

이 모드는 가장 널리 쓰이며 또 Tiny Core 를 사용할 때 추천하는 모드입니다. 

응용 프로그램은 영구 저장소의 **tce** 라는 디렉토리에 로컬로 저장됩니다. 예를 들면 지원되는 디스크 파티션 (ext2, ext3, ex4, vfat) 이 있습니다. 응용 프로그램은 재부팅할 때 선택해서 마운트할 수 있습니다. (포럼 및 wiki 의 **onboot.lst** 를 보도록 합니다.)  마운트된 응용 프로그램은 다른 사용을 위해서 RAM 을 절약합니다. 

**tce=xdyz** 를 부트 코드로 지정하지 않은 경우 Tiny Core 는 컴퓨터의 모든 드라이브를 검색해서 첫번째로 찾은 **/tce** 디렉토리에 확장들을 저장/로드합니다.

Tiny Core 는 앱 도구를 사용하여 응용 프로그램 확장을 이 **/tce** 디렉토리에 위치하고 이들을 "OnBoot" (부팅할 때 마운트) 또는 "OnDemand" (부팅할 때 마운트하진 않지만 특별한 메뉴 섹션을 만들어서 쉽게 접근할 수 있도록 하고 가능하다면 아이콘을 보이) 라고 표시합니다.

#### 1.9 복사 모드

복사 모드는 마운트 모드를 수정한 것입니다. 

선택한 응용 프로그램 확장을 마운트하는 대신 RAM 에 복사합니다. 응용 프로그램은 RAM 에 대량으로 로드 (**copy2fs.flg**), 선택한 것만 로드 (**copy2fs.lst**), 또는 마운트될 수 있습니다. 앱 (Apps) 프로그램은 설치/로드 옵션 (대량 복사, 선택 복사 등) 을 추적합니다. 부트 시간은 더 길어지는데 RAM 에 복사하는 것이 마운트하는 것보다 시간이 더 걸리기 때문입니다. 하지만 실행 시간 특히 첫 시작의 경우 훨씬 빨라집니다. 

복사 모드는 간단하게 말해서 RAM 을 더 얻기 위해 부트 시간이 더 필요합니다. - RAM 은 기본 모드의 실행 속도와 순수 마운트 모두의 영구 저장을 위해 필요합니다. (_이 부분은 다시 정리해야 합니다._)

복사 모드에서는 확장을 마운트할 수도 있고 RAM 에 복사할 수도 있음을 주목해야 합니다. 앱 프로그램이 사용자 선택을 추적하고 있기 때문에 이러한 유연성이 가능해집니다.

대량 로드를 선택하면, 즉 모든 확장을 RAM 에 로드하면, 스토리지를 언마운트할 수 있고, 시스템이 전원 손실로 인해서 파괴되지 않도록 할 수 있음에 주목합니다.

#### 1.10 백업 / 복구 & 다른 옵션

이 부분은 당장은 몰라도 될 것 같아서 넘어갑니다. 다음에 정리하도록 합니다. 

##### 1.10.1 백업 / 복구

##### 1.10.2 저장 홈

#### 1.11 정리

### 2 장. 설치 (Installing)

Core 의 설치는 세 부분으로 구성됩니다: 어떤 미디어에 있는 부트 로더, 어떤 미디어에 있는 메인 이미지 (커널과 **core.gz**), 그리고 어떤 미디어에 있는 **tce** 디렉토리가 그것입니다.

이들 모두는 같은 디스크에 있을 수도 있지만, 꼭 그럴 필요는 없습니다; 필요하다면 세 가지 모두 별도의 미디어에 있을 수 있습니다. 

> Core 설치는 완전한 이동이 가능하며, 설치 시스템으로부터 어떠한 설정도 읽지 않습니다. 
> 
> 이것은 한 시스템의 드라이브에 설한 것을 아무 문제없이 다른 시스템으로 옮길 수 있음을 의미합니다. 이는 CD 나 USB 로 부팅할 수 없는 노트북의 경우에 유용합니다.

#### 2.1. 공식 설치 프로그램 사용하기

이 부분도 지금은 필요없는 것 같아서 넘기도록 합니다. 

##### 2.1.1. 

##### 2.1.2. 

##### 2.1.3. 

##### 2.1.4. 

##### 2.1.5

#### 2.2. Windows 에서 core2usb 사용하기

이 부분도 넘깁니다.

#### 2.3. 수동 설치하기

수동 설치는 모든 리눅스 배포판에서 수행할 수 있습니다. 숙련된 사용자의 경우 CD 를 굽거나 설치 프로그램을 이용하는 것보다 더 빠르게 설치할 수 있습니다. 

정확한 설치 단계는 프로그램과 호스트 배포판의 선택에 따라 많이 달라지므로 여기서는 일반인 부분만 다룹니다. 

##### 2.3.1. 1 단계: 분할 & 포맷하기

**BIOS 설치**

원하는 프로그램으로 대상 디스크를 보통 방식으로 분할합니다: GUI 의 경우 **Gparted** 를 명령줄에서는 **cfdisk** 를 추천합니다; 주요 배포판에서는 둘 모두 가능하도록 해야 합니다. 

분할된 부분은 리눅스 파일 시스템으로 포맷해야합니다. 일반 용도로는 **ex4** 를 추천합니다. 대상 시스템이 USB 이거나 쓰기가 제한된 미디어인 경우 **ext2** 를 대신해서 쓸 수도 있는데 저널링 파일 시스템이 무결성을 유지하기 위해 추가로 쓰기를 할 수 있도록 하기 때문입니다.

대상이 보통의 하드 디스크인 경우, 스왑 분할을 만들고 포맷하는 것을 추천합니다. (_좀 더 정리하도록 합니다._)

> XFS 같은 더 별종 파일 시스템을 사용하려면 XFS 파티션에 접근하기 위해 XFS 지원을 로드하는 리마스터 또는 다른 방법이 필요합니다. (_정리가 필요합니다._)

**UEFI 설치**

원하는 프로그램을 사용하여 GPI EFI 부트 분할과 일반 분할을 만듭니다: GUI 의 경우 **Gparted** 를 명령줄에서는 **gdisk** 를 추천합니다; 주요 배포판에서는 둘 모두 가능하도록 해야 합니다. 

EFI 분할은 **vfat** 으로 포맷해야하며 일반 분할은 리눅스 파일 시스템으로 포맷해야 합니다.

> 오래된 애플 기기는 보통 32-비트 EFI 를 사용하는 반면 현대의 애플 기기와 PC 하드웨어는 64-비트 (U)EFI 를 사용합니다. 이것은 64-비트 (U)EFI 설치를 하려면 **core64** 나 **corepure64** 를 사용할 필요가 있음을 의미합니다.

##### 2.3.2. 2 단계: 파일 (Files)

최신 Core 파일은 편의를 위해 분리가능해서 - ISO 파일에서 압축을 풀 필요가 없습니다. 가장 가까운 미러 사이트의 **release/distribution_files** 디렉토리에서 **core.gz** 와 **vmlinuz** 를 내려받습니다. 주요 미러 사이트의 링크는 http://repo.tinycorelinux.net/4.x/x86/release/distribution_files/ 입니다.

커널과 initrd 의 위치는 보통 대상 분할의 **/boot** 밑이지만 어느 위치에 둬도 상관없습니다. 

확장을 따로 유지하라면 대상 분할에서 **tce** 라는 루트 디렉토리를 만듭니다.

##### 2.3.3. 3 단계: 부트 로더 (Bootloader)

마지막으로 대상 디스크의 MBR 에 부트 로더를 설치하고 커널과 initrd 를 카리키도록 해야 합니다. 

BIOS 설치에서는 syslinux 계열인 lilo, grub 0.x 및 grub 2 가 잘 작동함을 테스트했습니다. UEFI 설치에서는 grub 2 만 테스트했습니다.

보통의 부팅에서는 어떤 부트 코드도 추가할 필요가 없습니다. - **tce** 디렉토리의 위치는 자동으로 감지됩니다. 여러 개의 **tce** 디렉토리를 가지는 것이 적절한 경우에는 부트 코드가 있는 곳을 지정하는 것이 좋습니다.

USB 나 SD 카드처럼 탈착 가능하고 느린 미디어에서는 **waitusb** 부트 코드를 추가하는 것이 필요할 수 있습니다. 이것은 Core 가 주어진 시간만큼 기다리게 해서 느린 장치가 등록되도록 시간을 벌어 주고 장치가 나타나자마자 분할 꼬리표나 UUID 를 제출할 수 있게 합니다. 

문법의 경우 5 초 동안 기다리려면 `waitusb=5` 처럼 하면 되고, 20 초 동안 기다린 후 "mydisk" 라는 분할 꼬리표를 보이려면 `waitusb=20:LABEL=mydisk` 라고 하면 됩니다.

마지막으로 커널의 부트 출력을 제한하려고 **quiet** 부트 코드를 쓸 수도 있습니다.

보통의 grub 0.97 설정 파일은 다음과 같습니다:

```
default 0
timeout 10

title Core
root (hd0, 0)
kernel /boot/vmlinuz quiet waitusb=5
initrd /boot/core.gz
```

마찬가지로 보통의 grub 2 설정 파일 (분할의 UUID 가 교체된 경우) 는 다음과 같습니다:

```
search --no-floppy --fs-uuid --set=root "fdsf-gt434"

menuentry "Core" {
	linux /boot/vmlinuz quiet waitusb=5
	initrd /boot/core.gz
}
```

### 3 장. GUI 를 통한 기본 패키지 관리 

일종의 프로그램 설치 삭제 관리자인 것 같습니다. 당장은 필요 없는 것 같습니다.

### 4 장. CLI 를 통한 기본 패키지 관리

이것은 명령줄에서 사용하기 위한 일종의 프로그램 설치 삭제 관리자인 것 같습니다. 당장은 필요 없는 것 같습니다.

리눅스의 `apt` 나 `yum` 과 같은 것이라 생각하면 될 것 같습니다.

### 5 장. 기본 시스템 업데이트하기

이것도 당장은 필요없는 것 같습니다. 

### 6 장. 확장 업데이트하기

이부분도 당장은 필요없는 것 같습니다. 설치된 확장들을 업데이트하는 방법을 다루는 것 같습니다. 

### 7 장. 저장하기 (?)

필요는 한 것 같은데 일단 이후에 보도록 합니다. 

### 8 장. 확장 관리하기 

조금은 알아야할 것 같습니다. 

### 9 장. 가상화

이 부분도 당장 필요없는 것 같습니다. 

### 10 장. 부트 코드 설명

#### 10.1 tce - 확장 디렉토리

### 18 장. TCZ 포맷 (The TCZ format)


### 19 장. 부팅 과정 (The boot process)

### 23 장. 자동화된 네트워크 설치관리자 (Automated newwork installer)

### 27 장. 네트워크 부팅 (Network booting)









