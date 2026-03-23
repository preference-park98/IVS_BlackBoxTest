# IVS_BlackBoxTest

## 파일별 설명 먼저 보기
이 저장소는 여러 개의 CAPL 노드로 입력/요청 메시지를 생성하고, `tfs_IVS_Project.can`에서 이를 이용해 진단 테스트를 수행하는 구조로 되어 있습니다.

### 1) `tfs_IVS_Project.can`
이 저장소의 메인 테스트 파일입니다.

- `MainTest()`에서 실행할 테스트케이스를 호출
- `DTC_Clr()`로 fault clear 요청 전송
- `Fault_Req()`로 fault status 요청 전송
- `Battery_Percent_Low()`, `Battery_Charging_Error()` 테스트 포함
- 실제 판정 로직과 timing/erase 검증이 들어 있는 핵심 파일

### 2) `BMS.can`
배터리 관련 입력을 송신하는 CAPL 노드입니다.

- `BATT_01_10ms` 주기 송신
- `Batt_Percent`, `Batt_Chg_Sts`, `Batt_Voltage` 제어
- enable sysvar에 따라 시험값/기본값 전환

### 3) `BRK.can`
브레이크/가속/차속 입력을 송신하는 CAPL 노드입니다.

### 4) `ENG.can`
점화 및 엔진 상태를 송신하는 CAPL 노드입니다.

### 5) `STR.can`
조향각 입력을 송신하는 CAPL 노드입니다.

### 6) `UDS.can`
fault status / clear, version request를 보내는 진단 요청 노드입니다.
