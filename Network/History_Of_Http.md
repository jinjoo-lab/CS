# History Of HTTP

[노션](https://daisy-atmosphere-561.notion.site/History-Of-HTTP-c4faa6eba0784f8d896b98892b6518be?pvs=4)

### HTTP/0.9

- **원 - 라인 프로토콜**
- 초기의 HTTP에는 버전 번호가 없었다. 나중에 이전 버전과 구분하기 위해 0.9라는 버전을 추가했다.

특징

- 요청은 단일 라인으로 구성
- 리소스에 대한 경로로 가능한 메서드는 `[GET](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/GET)`이 유일
- 즉 파일 내용 자체 전송이 주 목적
- HTTP 헤더가 없다.

```
GET /mypage.html
```

### HTTP/1.0

- 상태 코드 추가
- **HTTP의 헤더 개념**이 요청과 응답에 도입
- 메타데이터 전송이 가능
- **1 Request & 1 Response Per 1 Connection**
    - 요청과 응답이 발생할 때마다 TCP 연결을 해야 한다.
    - 서버의 부하와 비용이 심한 방법

```
GET /mypage.html HTTP/1.0
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)

200 OK
Date: Tue, 15 Nov 1994 08:12:31 GMT
Server: CERN/3.0 libwww/2.17
Content-Type: text/html

<HTML>
A page with an image
  <IMG SRC="/myimage.gif">
</HTML>
```

### HTTP/1.1

- **Persistent Connection (지속 연결)** 도입
    - 응답을 하더라도 바로 TCP연결이 종료되지 않는다. Timeout 개념 도입
- **파이프라이닝 기법** 추가
    - 통신 지연 시간 단축
- 캐시 제어 메커니즘 도입
- `[Host](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers/Host)` 헤더 덕분에, 동일 IP 주소에 다른 도메인을 호스트하는 기능이 서버 배치가 가능

## HTTP/2.0

> 기존 HTTP/1.1 버전의 성능 향상에 초점 + 확장
>

### Binary Protocol & Header Compression

> **데이터 파싱 및 전송 속도 증가 , 오류 발생 가능성 감소**
>
1. HTTP 메시지 전송 방식의 변화 : 텍스트 프로토콜이 아닌 **바이너리 프로토콜**
    - 헤더와 데이터를 바이너리 형식으로 인코딩하여 전송
    - HTTP 1.0 : 헤더와 바디를 **`\n`**의 개행 문자로 구분
    - HTTP 1.1 : 헤더와 바디를 계층화

**데이터 인코딩**

- **HTTP/1.1:** 주로 텍스트 형식으로 데이터를 인코딩 , 헤더와 본문은 일반적으로 텍스트로 표현되며, MIME 타입과 함께 전송
- **HTTP/2.0:** 데이터는 이진 형식으로 인코딩됩니다. 헤더와 데이터는 이진으로 인코딩되어 전송되므로, 사람이 직접 읽기 어렵습니다.

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/4094263c-ce06-46ca-bcf0-239798d631a9)

**헤더 압축**

- **HTTP/1.1:** 헤더는 텍스트 형식으로 전송되며, 중복되는 헤더 필드들이 매번 전송되기 때문에 오버헤드가 발생할 수 있습니다.
- **HTTP/2.0:** 헤더 압축이 도입되어 **중복 데이터를 최소화**하고 효율적으로 전송 , 헤더는 이진 형식으로 전송되고 압축 알고리즘에 의해 압축된 상태로 전송되어 더 적은 대역폭을 사용

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/c3cbd2ca-6f75-4b0e-b171-116f03eddbba)

- 연속적으로 요청되는 HTTP 메시지들 : 헤더값에 중복 발생
- Static Dynamic Table 개념 도입 → 중복 헤더 검출
    - 중복시 헤더의 index만 전송
    - 호프만 인코딩(Huffman Encoding) 기법을 사용하는 HPACK 압축 방식

### Stream & Frame

> 기존 HTTP 1.1은 메시지라는 단위를 사용했다. 하지만 HTTP 2.0에는 Stream과 Frame의 단위가 추가되었다.
>
- Frame : HTTP/2.0에서 통신의 최소 단위
- Stream : Connection 내에서 양방향으로 메시지를 주고 받는 흐름

> HTTP Request → Frame → Message → 특정 Stream에 종속 → **여러개의 Stream이 1개의 Connection에 속하게 되는 구조**
>

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/d86db7dd-ebed-496e-b313-0faa200e79eb)

- 하나의 Connection 내에서 스트림들이 병렬적으로 처리 → **속도 향상**

### Multiplexing

> 하나의 Connection을 통해 여러 데이터 스트림을 동시에 전송하는 기술
>
- 다수의 요청을 **병렬적으로 처리** 가능
- 요청 순서에 상관 없이 먼저 완료한 순서대로 전송
- **HOL Blocking 해결**

### Stream Prioritization

> 리소스간 우선 순위 설정 가능
>
- 여러 데이터 프레임들이 Multiplexing되면서 하나의 연결 내에서 여러개의 요청과 응답 순서의 개념이 모호해졌다.
- 클라이언트는 **우선순위 지정 트리** 사용
    - 스트림에 식별자 설정
        - 각각의 스트림은 1-256 까지의 가중치를 갖음
        - 하나의 스트림은 다른 스트림에게 명확한 의존성

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/25be4460-d975-4b23-a309-233a167dddfa)

- 우선순위 적용시
    1. 클라이언트는 서버에 요청 시 요청 시 **자원에 가중치 우선순위 지정** 후 전송
    2. 서버는 우선순위에 따라 **대역폭 설정**
    3. 각 프레임에는 고유 식별자 존재 → 인터리빙 등 기술 사용 가능

### Server Push

> 서버가 요청 없이 클라이언트에 콘텐츠를 능동적으로 전송하는 기능
>
- 필요한 리소스를 다시 요청하여 발생하게 되는 트래픽 , 회전 지연을 줄인다.
- 기본적으로 클라이언트는 HTML 문서를 요청하고, 서버는 HTML을 보낸 후에 해당 HTML에 필요한 리소스들(예: 이미지, 스타일시트, 스크립트)을 클라이언트에게 보냅니다.

### HOL Blocking에 대한 고찰

> Binary Frame으로 Message를 분할하고 Multiplexing을 통해 HTTP내에서 HOL Blocking을 감소시켰다.
>
- HOL Blocking이 발생하는 원인은 뭘까?
    - 연결을 통한 직렬 전송 (순차적 처리) → **어 이건 HTTP/2.0에서 해결했잖아 ?**

### TCP HOL Blocking

> 일부 데이터가 지연되면 그 뒤의 모든 데이터가 기다리게 되는 현상
>
- 데이터가 전송되는 동안 중간에 패킷이 손실되거나 재전송되어 지연이 발생하는 경우
- **TCP는 데이터의 순서를 보장**하기 위해 패킷을 순차적으로 전송

> 결국 HTTP 자체에 대한 HOL Blocking을 해결했다 하더라도 근본적으로 전송 계층에서 TCP를 사용한다면 **TCP의 HOL Blocking은 HTTP에서 해결할 수 없다.**
>

## HTTP/3.0

> QUIC 프로토콜이 TCP/IP 4계층에도 동작시키기 위해 설계된 것이 바로 HTTP 3.0
>

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/560fb315-35e0-41d9-b18e-68b1f8177744)

## QUIC

> 구글에서 만든 **UDP 기반의 전송 계층 프로토콜**
>

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/7456abe1-dc3a-4c62-8867-30234b3d60fc)

### Why UDP

1. **Low Latency**
    - TCP는 HandShake 과정에 있어 부가적인 RTT를 필요로 한다. 연결 설정 자체가 주는 지연을 UDP는 필요로 하지 않는다.
2. **Multiplexing**
    - UDP는 연결 설정이 필요 없이 여러 데이터 스트림을 동시에 전송
3. **Header**
    - TCP의 Header는 UDP의 Header보다 무겁다. 2.0에서 Header 압축으로 성능 향상을 이루어냈지만 여전히 무겁다.

---

1. **독립 스트림**

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/f7d0fc5b-908c-485f-a9b5-e270615de67b)

- 요청별로 다른 스트림을 사용
    - 특정 요청이 지연 처리되더라도 다른 요청은 처리된다.

2. **RTT 최소화**
    - 첫 연결 설정에서 필요한 정보 + 데이터 전송
        - QUIC은 참고로 첫 연결 설정에 TLS 연결까지 완료 (내부 탑재)
    - 연결 설정을 캐싱하여 다음 연결 때 재사용
    - **Connection UUID**라는 고유 식별자로 서버와 연결
        - 인터넷 권역이 바뀌더라도 부가적인 TLS 연결은 불필요하다.
            - 기존에 IP와 Port번호로 구분하던 체계를 변경

![image](https://github.com/jinjoo-lab/SSAFY_CS_Study/assets/84346055/e8a1e263-bdaa-4f7b-9888-1c24450046f2)

### 참고

[[10분 테코톡] 🧃쿨라임의 HTTP/1.1, HTTP/2, 그리고 QUIC](https://www.youtube.com/watch?v=xcrjamphIp4)
[[10분 테코톡] 포이의 HTTP1.1, HTTP2, 그리고 QUIC](https://www.youtube.com/watch?v=Zyv1Sj43ykw)
