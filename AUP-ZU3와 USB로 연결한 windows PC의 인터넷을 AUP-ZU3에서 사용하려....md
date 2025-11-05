AUP-ZU3 보드를 USB 연결을 통해 Windows PC의 인터넷에 연결하여 PyVISA 종속성 패키지를 설치하려면, **별도의 AUP-ZU3 패키지 설치는 필요하지 않으며,** 대신 호스트 PC에서 **네트워크 연결 공유(브리징) 설정**을 통해 AUP-ZU3의 네트워크 모드를 DHCP 클라이언트로 전환해야 합니다.1

AUP-ZU3는 기본적으로 DHCP 서버 모드로 작동하여 호스트 PC에 IP 주소(예: 192.168.3.1)를 할당하며, 이로 인해 외부 인터넷에서 격리됩니다.1 이 격리를 해제해야만 PyVISA의 USB 제어 기능에 필수적인 시스템 라이브러리(libusb) 및 Python 바인딩(pyusb)을 설치할 수 있습니다.1

다음은 AUP-ZU3에서 인터넷 접근성을 확보하고 필수 패키지를 설치하기 위한 가이드입니다.

---

## **AUP-ZU3 인터넷 연결 및 PyVISA 필수 패키지 설치 가이드**

### **I. AUP-ZU3의 인터넷 접근 문제 해결 (호스트 PC 설정)**

PYNQ 보드가 apt-get 및 pip 명령을 사용하여 외부 인터넷에서 패키지를 다운로드할 수 있도록 하려면, Windows 호스트 컴퓨터가 PYNQ 보드에 인터넷 연결을 공유(네트워크 브리징)하도록 설정해야 합니다. 이는 PYNQ 보드를 **DHCP 클라이언트 모드**로 전환하여 인터넷에 연결되도록 하는 가장 일반적인 방법입니다.1

**참고:** 이 설정은 Windows 운영체제에서 수행해야 하는 복잡한 절차이며, PYNQ 보드 자체에 설치해야 하는 패키지는 없습니다.

### **II. PyVISA USB-TMC 제어를 위한 필수 패키지 설치**

인터넷 연결이 활성화되면, PyVISA-Py가 USB 장치(USB-TMC)와 통신하는 데 필요한 시스템 및 Python 바인딩 라이브러리를 설치해야 합니다. USB 제어는 \*\*PyVISA-Py $\\rightarrow$ PyUSB $\\rightarrow$ 시스템 libusb\*\*의 3계층 소프트웨어 스택에 의존합니다.1

#### **1\. 시스템 libusb 라이브러리 설치**

AUP-ZU3는 데비안 기반의 임베디드 리눅스 환경을 사용하므로, apt-get 패키지 관리자를 사용하여 libusb 개발 파일을 설치해야 합니다.

| 패키지 | 기능 | 설치 방법 |
| :---- | :---- | :---- |
| libusb-1.0-0-dev | PyUSB가 리눅스 커널과 상호 작용하는 데 필요한 시스템 수준 라이브러리 1 | sudo apt-get update 및 sudo apt-get install libusb-1.0-0-dev |

**설치 단계 (Jupyter Terminal 또는 SSH에서 실행):**

1. 패키지 목록 업데이트:  
   인터넷 연결이 확보된 후, 패키지 목록을 최신 상태로 동기화합니다.  
   Bash  
   sudo apt-get update

2. libusb 개발 파일 설치:  
   USB 제어에 필수적인 시스템 라이브러리를 설치합니다.  
   Bash  
   sudo apt-get install libusb-1.0-0-dev

   이 과정에서 인터넷을 통해 파일이 다운로드됩니다.

#### **2\. PyUSB Python 바인딩 설치**

libusb가 설치된 후, PyVISA-Py가 USB 장치와 통신하는 데 사용하는 Python 바인딩인 pyusb를 설치해야 합니다.1

| 패키지 | 기능 | 설치 방법 |
| :---- | :---- | :---- |
| pyusb | PyVISA-Py와 libusb 간의 인터페이스 역할을 하는 Python 바인딩 1 | sudo pip3 install pyusb |

**설치 단계 (Jupyter Terminal 또는 SSH에서 실행):**

1. **pyusb 설치:**  
   Bash  
   sudo pip3 install pyusb

### **III. PyVISA-Py 백엔드 설치 (선행 확인)**

PyVISA와 순수 Python 기반 백엔드인 pyvisa-py는 일반적으로 PYNQ 이미지에 기본적으로 설치되어 있거나, 설치가 가장 쉽습니다. 만약 누락된 경우, USB-TMC를 포함한 모든 PyVISA 통신에 필수적이므로 설치해야 합니다.1

**설치 단계 (Jupyter Terminal 또는 SSH에서 실행):**

Bash

sudo pip3 install \-U pyvisa pyvisa-py

### **요약**

AUP-ZU3에서 USB를 통한 Windows PC 인터넷 접근을 설정하는 것은 **소프트웨어 패키지 설치보다는 호스트 PC의 네트워크 브리징 설정**에 의존합니다.1 일단 인터넷 접근이 성공적으로 이루어지면, PyVISA USB-TMC 통신을 위해 다음 두 패키지를 설치해야 합니다.

1. **시스템 종속성:** libusb-1.0-0-dev (APT 설치)  
2. **Python 바인딩:** pyusb (PIP 설치)