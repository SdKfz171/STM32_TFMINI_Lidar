# STM32_TFMINI_Lidar

TFMINI 라이다 모듈의 간단한 라이브러리를 제작하였다.

모듈의 전격 전압은 5V이며 통신 방식은 UART이다.

기본 통신관련 설정값은 115200에 8비트 데이터 비트, 1비트 정지 비트, 패리티는 없다.

Byte0    Byte1    Byte2    Byte3    Byte4      Byte5      Byte6    Byte7    Byte8

0x59     0x59     Dist_L   Dist_H   Strength_L Strength_H Mode     0x00     CheckSum

통신 프레임은 총 9개이며 각각의 프레임은 1바이트이다.

첫 프레임과 두번째 프레임은 HEADER값 0x59가 위치한다.

세번째와 네번째는 거리값이 위치하는데 세번째 프레임은 실제 거리값의 하위 8비트이고, 네번째는 상위 8비트이다.

그러므로 실제 거리값은 세번째 프레임 값에 네번째 프레임 X 0x100(256)한 값을 더 한 값이다.

같은 원리로 다섯번째, 여섯번째의 신호 세기값도 구할 수 있다.

일곱번째 프레임에는 모드값이 위치한다.

모드에는 단거리 모드와 장거리 모드가 존재하는데 신호 세기(거리)에 따라 자동으로 전환된다.

15X 버전 기준으로 0x02는 단거리, 0x07은 장거리인데, 16X 버전 기준으로는 0x03이 단거리, 0x07이 장거리이다.

여덟번째는 빈 프레임이고 마지막 프레임은 체크섬이 위치한다.

체크섬은 앞에서 부터 8개의 프레임의 값을 모두 더한 값의 하위 8비트의 값이다.

보통 통신이 제대로 되었는지 확인하기위해 사용된다.
