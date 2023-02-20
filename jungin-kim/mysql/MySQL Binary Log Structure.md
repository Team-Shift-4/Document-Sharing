[toc]



# MySQL Binary Log

이 문서는 MySQL 8.0.32(MySQL 8.0)을 기준으로 작성되었습니다.

MySQL의 Binlog의 단위는 Event입니다.

예시로 `INSERT INTO test VALUES(1)`이라는 DML을 실행시킬 시 `ANONYMOUS_GTID_LOG_EVENT`, `QUERY_EVENT`, `TABLE_MAP_EVENT`, `WRITE_ROWS_EVENT`, `XID_EVENT`가 실행됩니다.

MySQL은 Write Ahead Logging이 아니라 선택에 의해 Log를 작성할 수 있습니다.
`COMMIT`하지 않을 시 LOG에 남지 않습니다(따라서 `ROLLBACK`은 남지 않습니다).
위와 같은 이유로 MySQL은 대량의 처리보다 소량에 처리에 유리하다는 특징을 가집니다(Memory에 적재하기 때문에 대량의 Transaction을 처리하기 힘들기 때문입니다.).

## MySQL Binary Log Structure

### Architecture

MySQL의 Binlog는 크게 Common-Header, Post-Header, Body, Footer라는 4가지 구조로 이루어집니다.

#### 1. Common-Header

공통 양식입니다(19 Byte).

| Name                 | Format | Description                                                  |
| -------------------- | :----: | ------------------------------------------------------------ |
| `when`               | 4 Byte | Unix Timestamp이며 이 Query가 시작했을 시간을 나타냅니다.    |
| `type_code`          | 1 Byte | 아래의 [Event Type](#event-type)에 해당하는 코드입니다.      |
| `unmasked_server_id` | 4 Byte | 이 이벤트가 생성된 Server의 ID입니다.                        |
| `data_written`       | 4 Byte | 이 이벤트의 길이입니다(Common_Header_Len + Post-Header_Len + Body_Len). |
| `log_pos`            | 4 Byte | 다음 Master Server Binary Log의 위치                         |
| `flags`              | 2 Byte | 버전에 따른 Binary Log Flag                                  |

#### 2. Post-Header

Event Type에 따라 다른 고정 크기 Header입니다.
아래 [Event Type](#event-type) 참조

#### 3. Body

가변 길이의 데이터입니다.
같은 Event Type을 가져도 내용과 길이가 다를 수 있습니다.

#### 4. Footer

선택 양식입니다(0 or 4 Byte).

| Name           | Format | Description                                                  |
| -------------- | :----: | ------------------------------------------------------------ |
| `checksum_alg` | 4 Byte | Checksum(CRC32)를 사용할 때 추가로 Event 뒤에 붙으며 Common-Header의 `data_written`을 수정함 |

해당 Event의 Checksum 값은 이벤트를 Binary Log에 기록하기 직전에 계산합니다.
Checksum 값인 Footer는 이벤트의 일부로 간주되도록 이 이벤트의 `data_written` Field가 조정됩니다(+ `BINLOG_CHECKSUM_LEN`).

### Event Type

> [mysql-server/libbinlogevents/include/binlog_event.h 참고](https://github.com/mysql/mysql-server/blob/8.0/libbinlogevents/include/binlog_event.h)

| Event Name                  | Hexadecimal(Decimal) | Post Header Length(Byte) | Description                                                  |
| --------------------------- | :------------------: | :----------------------: | ------------------------------------------------------------ |
| `UNKNOWN_EVENT`             |       0x00(0)        |            -             | 존재할 수 없는 Event                                         |
| `START_EVENT_V3`            |       0x01(1)        |            56            | 2 + (`ST_SERVER_VER_LEN` = 50) + 4                           |
| `QUERY_EVENT`               |       0x02(2)        |            13            |                                                              |
| `STOP_EVENT`                |       0x03(3)        |            0             |                                                              |
| `ROTATE_EVENT`              |       0x04(4)        |            8             |                                                              |
| `INTVAR_EVENT`              |       0x05(5)        |            0             |                                                              |
| `SLAVE_EVENT`               |       0x07(7)        |            0             |                                                              |
| `APPEND_BLOCK_EVENT`        |       0x09(9)        |            4             |                                                              |
| `DELETE_FILE_EVENT`         |       0x0B(11)       |            4             |                                                              |
| `RAND_EVENT`                |       0x0D(13)       |            0             |                                                              |
| `USER_VAR_EVENT`            |       0x0E(14)       |            0             |                                                              |
| `FORMAT_DESCRIPTION_EVENT`  |       0x0F(15)       |            98            | `START_EVENT_V3_HEADER_LEN` + 1 + (`ENUM_END_EVENT` -1)      |
| `XID_EVENT`                 |       0x10(16)       |            0             |                                                              |
| `BEGIN_LOAD_QUERY_EVENT`    |       0x11(17)       |            4             |                                                              |
| `EXECUTE_LOAD_QUERY_EVENT`  |       0x12(18)       |            26            | `QUERY_HEADER_LEN` + (`EXECUTE_LOAD_QUERY_EXTRA_HEADER_LEN` = 13) |
| `TABLE_MAP_EVENT`           |       0x13(19)       |            8             |                                                              |
| `WRITE_ROWS_EVENT_V1`       |       0x17(23)       |            8             |                                                              |
| `UPDATE_ROWS_EVENT_V1`      |       0x18(24)       |            8             |                                                              |
| `DELETE_ROWS_EVENT_V1`      |       0x19(25)       |            8             |                                                              |
| `INCIDENT_EVENT`            |       0x1A(26)       |            2             |                                                              |
| `HEARTBEAT_LOG_EVENT`       |       0x1B(27)       |            0             |                                                              |
| `IGNORABLE_LOG_EVENT`       |       0x1C(28)       |            0             |                                                              |
| `ROWS_QUERY_LOG_EVENT`      |       0x1D(29)       |            0             |                                                              |
| `WRITE_ROWS_EVENT`          |       0x1E(30)       |            10            |                                                              |
| `UPDATE_ROWS_EVENT`         |       0x1F(31)       |            10            |                                                              |
| `DELETE_ROWS_EVENT`         |       0x20(32)       |            10            |                                                              |
| `GTID_LOG_EVENT`            |       0x21(33)       |            42            |                                                              |
| `ANONYMOUS_GTID_LOG_EVENT`  |       0x22(34)       |            42            |                                                              |
| `PREVIOUS_GTIDS_LOG_EVENT`  |       0x23(35)       |            0             |                                                              |
| `TRANSACTION_CONTEXT_EVENT` |       0x24(36)       |            18            |                                                              |
| `VIEW_CHANGE_EVENT`         |       0x25(37)       |            52            |                                                              |
| `XA_PREPARE_LOG_EVENT`      |       0x26(38)       |            0             |                                                              |
| `PARTIAL_UPDATE_ROWS_EVENT` |       0x27(39)       |            10            |                                                              |
| `TRANSACTION_PAYLOAD_EVENT` |       0x28(40)       |            40            |                                                              |
| `HEARTBEAT_LOG_EVENT_V2`    |       0x29(41)       |            ?             |                                                              |
| `ENUM_END_EVENT`            |       0x2A(42)       |            -             | End Marker(다른 수식에 이용)                                 |

### Packed Integer

Unsigend Integer를 효율적으로 나타내기 위한 특별한 형식입니다.
최대 9 Byte를 사용할 수 있지만 값이 작을 경우 1, 3, 4 Byte만 사용할 수도 있습니다.
첫 Byte의 값에 따라 읽는 방법이 달라집니다.

| First Byte  | Range                                                        | Description                             |
| ----------- | ------------------------------------------------------------ | --------------------------------------- |
| 0x00 ~ 0xFA | 0x00 ~ 0xFA(0~250)                                           | 1 Byte만 사용하며 첫 Byte가 해당하는 값 |
| 0xFC        | 0xFB ~ 0xFFFF(251 ~ 65535)                                   | 3 Byte를 사용하며 첫 Byte를 제외한 값   |
| 0xFD        | 0xFFFF ~ 0xFFFFFF(65535 ~ 16777215)                          | 4 Byte를 사용하며 첫 Byte를 제외한 값   |
| 0xFE        | 0xFFFFFF ~ 0xFFFFFFFFFFFFFFFF(16777215 ~ 18446744073709552000) | 9 Byte를 사용하며 첫 Byte를 제외한 값   |

## Event Type Struct

### START_EVENT_V3

### QUERY_EVENT

### STOP_EVENT

`STOP_EVENT`는 Common-Header밖에 존재하지 않습니다.

### ROTATE_EVENT

`ROTATE_EVENT`는 8 Byte의 Post-Header와 가변 길이의 Body로 이루어져 있습니다. 

<table><thead><tr><th colspan="16">41) OPCODE: ROTATE, POSITION: 4866</th></tr></thead><tbody><tr><td>cc</td><td>69</td><td>e4</td><td>63</td><td>04</td><td>01</td><td>00</td><td>00</td><td>00</td><td>29</td><td>00</td><td>00</td><td>00</td><td>02</td><td>13</td><td>00</td></tr><tr><td colspan="16">1</td></tr><tr><td>00</td><td>00</td><td>00</td><td>04</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>62</td><td>69</td><td>6e</td><td>2e</td><td>30</td></tr><tr><td colspan="3">1</td><td colspan="8">2</td><td colspan="5">3</td></tr><tr><td>30</td><td>30</td><td>30</td><td>30</td><td>33</td><td>d6</td><td>cb</td><td>e8</td><td>27</td><td colspan="7" rowspan="2"></td></tr><tr><td colspan="5">3</td><td colspan="4">4</td></tr></tbody></table>

| Num  | Name            | Length    | Description |
| ---- | --------------- | --------- | ----------- |
| 1    | Common-Header   | 19 Byte   |             |
| 2    | `position`      | 8 Byte    |             |
| 3    | `new_log_ident` | 가변 길이 |             |
| 4    | Footer          | 4 Byte    |             |

### INTVAR_EVENT

### SLAVE_EVENT

### APPEND_BLOCK_EVENT

### DELETE_FILE_EVENT

### RAND_EVENT

### USER_VAR_EVENT

### FORMAT_DESCRIPTION_EVENT

`FORMAT_DESCRIPTION_EVENT`는 Body 없이 Common-Header와 Post-Header로 구성되어 있습니다.

<table><thead><tr><th colspan="16">OPCODE: FORMAT DESCRIPTION</th></tr></thead><tbody><tr><td>29</td><td>45</td><td>e4</td><td>63</td><td>0f</td><td>01</td><td>00</td><td>00</td><td>00</td><td>7a</td><td>00</td><td>00</td><td>00</td><td>7e</td><td>00</td><td>00</td></tr><tr><td colspan="16">1</td></tr><tr><td>00</td><td>01</td><td>00</td><td>04</td><td>00</td><td>38</td><td>2e</td><td>30</td><td>2e</td><td>33</td><td>32</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td></tr><tr><td colspan="3">1</td><td colspan="2">2</td><td colspan="11">3</td></tr><tr><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td></tr><tr><td colspan="16">3</td></tr><tr><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td></tr><tr><td colspan="16">3</td></tr><tr><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>29</td><td>45</td><td>e4</td><td>63</td><td>13</td><td>00</td><td>0d</td><td>00</td><td>08</td></tr><tr><td colspan="7">3</td><td colspan="4">4</td><td>5</td><td colspan="4">6</td></tr><tr><td>00</td><td>00</td><td>00</td><td>00</td><td>04</td><td>00</td><td>04</td><td>00</td><td>00</td><td>00</td><td>62</td><td>00</td><td>04</td><td>1a</td><td>08</td><td>00</td></tr><tr><td colspan="16">6</td></tr><tr><td>00</td><td>00</td><td>08</td><td>08</td><td>08</td><td>02</td><td>00</td><td>00</td><td>00</td><td>0a</td><td>0a</td><td>0a</td><td>2a</td><td>2a</td><td>00</td><td>12</td></tr><tr><td colspan="16">6</td></tr><tr><td>34</td><td>00</td><td>0a</td><td>28</td><td>00</td><td>01</td><td>03</td><td>ab</td><td>18</td><td>a6</td><td colspan="6" rowspan="2"></td></tr><tr><td colspan="5">6</td><td>7</td><td colspan="4">8</td></tr></tbody></table>

| Num  | Name                    | Length  | Description |
| ---- | ----------------------- | ------- | ----------- |
| 1    | Common-Header           | 19 Byte |             |
| 2    | `binlog_version`        | 2 Byte  |             |
| 3    | `server_version`        | 50 Byte |             |
| 4    | `created`               | 4 Byte  |             |
| 5    | `common_header_len`     | 1 Byte  |             |
| 6    | `post_header_len`       | 41 Byte |             |
| 7    | `number_of_event_types` | 1 Byte  |             |
| 8    | Footer                  | 4 Byte  |             |

### XID_EVENT

`XID_EVENT`는 Post-Header 없이 8 Byte의 Body만 가지고 있습니다.

<table><thead><tr><th colspan="16">13) OPCODE: XID, POSITION: 1022</th></tr></thead><tbody><tr><td>1f</td><td>5f</td><td>e4</td><td>63</td><td>10</td><td>01</td><td>00</td><td>00</td><td>00</td><td>1f</td><td>00</td><td>00</td><td>00</td><td>fe</td><td>03</td><td>00</td></tr><tr><td colspan="16">1</td></tr><tr><td>00</td><td>00</td><td>00</td><td>38</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>00</td><td>7c</td><td>37</td><td>58</td><td>ed</td><td rowspan="2"></td></tr><tr><td colspan="3">1</td><td colspan="8">2</td><td colspan="4">3</td></tr></tbody></table>

| Num  | Name          | Length  | Description |
| ---- | ------------- | ------- | ----------- |
| 1    | Common-Header | 19 Byte |             |
| 2    | `xid`         | 8 Byte  |             |
| 3    | Footer        | 8 Byte  |             |



### BEGIN_LOAD_QUERY_EVENT

### EXECUTE_LOAD_QUERY_EVENT

### TABLE_MAP_EVENT

### WRITE_ROWS_EVENT_V1

### UPDATE_ROWS_EVENT_V1

### DELETE_ROWS_EVENT_V1

### INCIDENT_EVENT

### HEARTBEAT_LOG_EVENT

### IGNORABLE_LOG_EVENT

`IGNORABLE_LOG_EVENT`는 Common-Header밖에 존재하지 않습니다.

### ROWS_QUERY_LOG_EVENT

### WRITE_ROWS_EVENT

### UPDATE_ROWS_EVENT

### DELETE_ROWS_EVENT

### GTID_LOG_EVENT

### ANONYMOUS_GTID_LOG_EVENT

### PREVIOUS_GTIDS_LOG_EVENT

### TRANSACTION_CONTEXT_EVENT

### VIEW_CHANGE_EVENT

### XA_PREPARE_LOG_EVENT

### PARTIAL_UPDATE_ROWS_EVENT

### TRANSACTION_PAYLOAD_EVENT

### HEARTBEAT_LOG_EVENT_V2

## Data Type

| Code      | Name          | Description |
| --------- | ------------- | ----------- |
| 0x00(0)   | `DECIMAL`     |             |
| 0x01(1)   | `TINY`        |             |
| 0x02(2)   | `SHORT`       |             |
| 0x03(3)   | `LONG`        |             |
| 0x04(4)   | `FLOAT`       |             |
| 0x05(5)   | `DOUBLE`      |             |
| 0x06(6)   | `NULL`        |             |
| 0x07(7)   | `TIMESTAMP`   |             |
| 0x08(8)   | `LONGLONG`    |             |
| 0x09(9)   | `INT24`       |             |
| 0x0A(10)  | `DATE`        |             |
| 0x0B(11)  | `TIME`        |             |
| 0x0C(12)  | `DATETIME`    |             |
| 0x0D(13)  | `YEAR`        |             |
| 0x0E(14)  | `NEWDATE`     |             |
| 0x0F(15)  | `VARCHAR`     |             |
| 0x10(16)  | `BIT`         |             |
| 0x11(17)  | `TIMESTAMP2`  |             |
| 0x12(18)  | `DATETIME2`   |             |
| 0x13(19)  | `TIME2`       |             |
| 0x14(20)  | `TYPED_ARRAY` |             |
| 0xF3(243) | `INVALID`     |             |
| 0xF4(244) | `BOOL`        |             |
| 0xF5(245) | `JSON`        |             |
| 0xF6(246) | `NEWDECIMAL`  |             |
| 0xF7(247) | `ENUM`        |             |
| 0xF8(248) | `SET`         |             |
| 0xF9(249) | `TINY_BLOB`   |             |
| 0xFA(250) | `MEDIUM_BLOB` |             |
| 0xFB(251) | `LONG_BLOB`   |             |
| 0xFC(252) | `BLOB`        |             |
| 0xFD(253) | `VARSTRING`   |             |
| 0xFE(254) | `STRING`      |             |
| 0xFF(255) | `GEOMETRY`    |             |