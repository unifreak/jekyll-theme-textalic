---
category: Research
tags: [Draft]
usemath: [latex, ascii]
---





본 포스팅은 Azure Private 5G Core에 대한 공부 내용을 정리한 글입니다.



  안녕하세요. 제가 속해있는 경희대학교 이동통신연구실은 국내 대학 최초로 상용 Private 5G 테스트베드를 구축한 연구실입니다. LG CNS와 협업하여 Open5G의 코드를 이용해 5G Core를 구현하였는데, Azure에서 Private 5G Core 자습서가 있어 이를 공부해보았습니다.

공식 문서에서 표기되어 있는 학습 목표는 다음과 같습니다.

# 학습 목표

1. 프라이빗 모바일 네트워크 솔루션에서 Azure Private 5G Core의 역할을 설명

2. Azure Private 5G Core의 주요 이점을 나열

3. Azure Private 5G Core의 작동 방식을 설명

4. Azure Private 5G Core의 주요 기능을 나열

   

# 소개

  Azure Private 5G 코어는 IoT 애플리케이션의 성능 및 보안 요구 사항을 충족하기 위해 Azure Edge Platform에서 간단하고 안전한 Private 5G 코어 네트워크 배포를 제공하게 됩니다. 통신 서비스 제공 뿐 아니라, Platform Metric을 이용하여 원격으로 또는 Packet Core Dashboard를 사용하여 로컬로 Private Network 사이트를 모니터링 할 수 있습니다.

다음은 스마트팩토리에서의 아키텍쳐입니다.

![웨어하우스 IoT 시스템의 다이어그램.](../assets/img/2023-10-15-Azure_Private5G_Core/ap5gc-warehouse-example.png)



# Azure Private 5G Core란?

