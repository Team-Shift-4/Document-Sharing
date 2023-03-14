# Bigdata용 Oracle GoldenGate

📌 Oracle GoldenGate 21.1 기준으로 작성된 문서

# 개요

Oracle GoldenGate for Big Data는 명확히 정의된 Software Version과 호환되는 Handler와 같은 특정 구성 요소를 지원

# Oracle GoldenGate for Big Data 설치

## 지원 사항

### 인증과 System 요구사항 확인

1.   환경이 인증 요구 사항을 충족하는지 확인
2.   System 요구 사항 문서를 사용해 확인
3.   여러 제품 간 상호 운용성 확인

### 추가 지원 고려 사항

#### Plug형 Formatter

-   HDFS Handler는 모든 Plug형 Formatter를 지원
-   Plug형 Formatter는 HBase Handler에 적용할 수 없음
    Data는 독점 HBase Client Interface를 사용해 HBase로 Streaming됨
-   Kafka Handler는 모든 Plug형 Formatter를 지원
-   Kafka Connect Handler는 Plug형 Formatter를 지원하지 않음
    Kafka Connect Data Converter를 사용해 Data를 JSON이나 Avro로 변환 가능
-   Kinesis Streams Handler는 Oracle GoldenGate for Big Data User Guide의 [Using the Pluggable Formatters](https://docs.oracle.com/en/middleware/goldengate/big-data/21.1/gadbd/using-pluggable-formatters.html)주제에 설명된 모든 Pluggable Formatter를 지원
-   Cassandra, MongoDB, JDBC Handler는 Plug형 Formatter를 사용하지 않음

#### Extract를 사용한 Java Delivery

Extract를 사용한 Java Delivery은 지원되지 않음

#### MongoDB Handler

-   Handler는 원본 Table의 고유한 Row만 Replicate 가능
    원본 Table에 정의된 PK가 없고 중복 Row가 있는 경우 중복 Row를 MongoDB 대상으로 Replicate할 경우 중복 키 오류 발생과 함께 Replicat Process가 이상 종료
-   누락된 Update와 Delete는 감지되지 않으므로 무시
-   Sharded Collaction으로 Test되지 않음
-   Milli-Sec 정밀도의 Date, Time Data Type만 지원
-   Trail에 포함된 Data Type은 지원되지 않음(`datetime`, `timezone`)
-   최대 BSON Document 크기는 16MB, Record Size가 16MB를 초과할 경우 Handler가 Record를 복제할 수 없음
-   DDL 전송 불가
-   Truncate 불가

## 설치 준비

## 설치

## Oracle GoldenGate for Big Data(Classic) 시작

## 구성