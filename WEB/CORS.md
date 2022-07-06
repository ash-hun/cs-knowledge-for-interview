# CORS

### 개발자가 CORS에러를 만나는 경우

- PostMan, 스프링 등에선 URI요청이 정상적으로 보내진다.
- 웹사이트(크롬, 엣지, 사파리)등 브라우저(Front End)에서 발생
- 보안상의 이유로 JavaScript에서는, 오류의 상세정보에 접근 불가
- 상세정보는 브라우저의 콘솔에서 확인해야함

# CORS

### :Cross-Origin Resource Sharing (교차 출처 리소스 공유)

브라우저-서버 간 안전한 **교차출처 요청**과 **데이터 전송**을 지원하는 체제

- **웹 애플리케이션**은 자신, 또는 다른 출처(도메인, 프로토콜,포트)에서 **리소스**를 요청한다.
- 외부 출처의 자원에 접근할 때, `교차 출처 HTTP요청`을 실행한다.
- ‘교차 출처 HTTP요청’은 외부 출처의 자원에 접근할 수 있는 **권한을 부여**하며,
- 이를 `추가 HTTP헤더`를 사용해 **브라우저**에 알리는 것이 `CORS`이다.

# SOP

### :Same-Origin Policy (동일 출처 정책)

 **서로 다른 출처**의 리소스가 상호작용하는 것을 제한하는 `보안방식`

`보안 상`의 이유로, **브라우저**는 **스크립트**에서 시작한 ‘교차 출처 HTTP 요청’을 **제한**한다.  

 잠재적으로 해로울 수 있는 문서를 분리해 공격받을 경로를 줄임.

- 동일한 출처란?
    
    두 URL의 `프로토콜` / `포트` / `호스트` 가 모두 같아야 동일한 출처
    
    EX) URL `http://store.company.com/dir/page.html`의 출처를 비교한 예시
    |url|결과|이유|
    |---|---|---|
    |http://store.company.com/dir2/other.html|성공|경로만 다름|   
    |http://store.company.com/dir/inner/another.html|성공|경로만 다름|   
    |https://store.company.com/secure.html|실패|프로토콜 다름|   
    |http://store.company.com:81/dir/etc.html|실패|포트 다름 (http://는 80이 기본값)|   
    |http://news.company.com/dir/other.html|실패|호스트 다름|   
    

이 정책을 따르는 API를 사용하는 웹 애플리케이션이 다른 출처의 리소스를 불러오려면, 

그 출처에서 올바른 CORS 헤더를 포함한 응답을 반환해야 한다. 

EX) 동일 출처 정책을 따르는 예시

- XMLhttpRequest
- Fetch API   

#### 필요성

- 크롬에서 방문한 사이트를 못믿음
- 다른 사이트 방문 시 쿠키, 세션 등에 저장되어있는 인증정보를 탈취해갈 가능성 있음
- 원래 브라우저 간의 리소스 공유는 막아지는게 원칙이었는데, 웹 생태계가 발전함에 따라 필요성을 느껴 만들어진 것임

![Untitled](https://user-images.githubusercontent.com/67628725/177526301-6c3c2922-505e-46d9-b98f-60aeb387c026.png)



## CORS에러 해결법

BE에서 허락할 다른 출처들(허용할 사이튿들)을 미리 명시함 

CORS 옵션을 넣어두는 방법은 프레임워크마다 명시되어있음 

## CORS기능 기본동작순서

1. [FE]요청에 ORIGIN 이라는 header(받는 쪽의 ip주소, 사용할 프로토콜, 옵션) 추가
2. [BE] 서버는 header에 지정된 `Acces-Control-Allow-Origin` 정보(허용된 url)을 실어보냄 
3. [FE] 비교 후 안전한 요청으로 판단 → 응답 보내줌 

## 1. Simple Request

- `Get`, `Post` 등의 요청
- 데이터에 영향을 미치지 않는 요청

## 2. Preflight Request

- `Put`, `Delete` 등의 요청
- 서버 데이터에 영향을 미치는 요청
    
    ### 추가 동작
    
    1. 브라우저가 `OPTIONS` 메서드로, 원하는 요청을 `사전전달 (preflight)`
    2. 서버가 실제요청울 허가(재요구) 
        1. 필요에 따라 인증정보(`쿠키`, `http인증`) 함께 요구 

## 3. Credentialed Request

- `토큰` 등 사용자 식별정보
- 더 엄격한 기준 적용
- [FE]  요청의 option에 `credentials` 항목을 true로 세팅
- [BE]  `Access-Control-Allow-Credentials` 항목을 true로 세팅
    
      출처-웹페이지 주소 정확히 명시 (와일드 카드 X)
