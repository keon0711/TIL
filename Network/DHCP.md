## DHCP
- Dynamic Host Configuration Protocol의 약자
- LAN 내 장치의 **IP를 자동으로 할당하는 클라이언트/서버 프로토콜**
- DHCP 서버가 작업을 처리하지만, 일반적으로 라우터와 기본 게이트웨이가 담당한다.

##  IP 주소 자동 할당 절차
1. **DHCP Discover**:클라이언트가 DHCP 서버를 찾기 위해 브로드캐스트
2. **DHCP Offer**: DHCP 서버가 메시지를 받으면, 할당 가능할 수 있다는 메시지를 회신
3. **DHCP Request**: 클라이언트가 응답한 DHCP 서버에 IP 주소를 요청
4. **DHCP Ack**: DHCP 서버가 필요한 정보를 발행

## DHCP가 제공하는 정보
- IP 주소, 서브넷 마스크, 기본 게이트웨이등