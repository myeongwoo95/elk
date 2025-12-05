# 1. 로그의 기초

### Checked Exception vs Unchecked Exception
- Checked Exception: 컴파일시점에 예외처리를 강제함
- Unchecked Exception: 컴파일시점에 예외처리를 강제하지 않음

### Checked Exception, Unchecked Exception 예외 생성
- Checked Exception -> extends Exception 
- Unchecked Exception -> extends RuntimeException

### Checked Exception, Unchecked Exception 언제 사용?
- 이미 정의되어있는 예외들은 정의된 대로 사용
- 새로운 예외를 직접 정의하는 경우에는 Unchecked Exception(RuntimeException) 권장
  - Checked Exception은 컴파일 시점에 예외처리를 강제하기때문에 try-catch나 throws 처리를 해줘야한다. -> 번거로움
- Checked Exception을 사용하는 경우에는 예외 핸들링을 할 수 있는 방법이 명확한 경우, 즉 에러를 바깥으로 던져줄 필요가(사용자에게 알려준다거나) 없을 경우 사용하면 좋다.

### 로그레벨
#### 1. TRACE
- 가장 세부적인 수준의 로그로, 코드의 세밀한 실행 경로를 추적할 때 사용합니다. 시스템 내부 흐름을 전부 추적해야 하는 경우에만 제한적으로 사용합니다.

#### 2. DEBUG
- 디버깅 목적의 로그로, 개발 중 코드의 상태나 흐름을 이해하기 위해 사용합니다. 운영 환경에서는 일반적으로 비활성화합니다.

#### 3. INFO
- 시스템의 정상적인 운영 상태를 나타내는 정보성 로그입니다. 주요 이벤트나 상태 변화를 기록하며 운영 상황을 파악하는 데 유용합니다.

#### 4. WARN
- 잠재적으로 문제가 될 수 있는 상황을 나타내지만, 시스템 운영에는 즉각적인 영향이 없을 때 사용합니다. 예방적 조치가 필요한 상황을 파악하는 데 적합합니다.

#### 5. ERROR
- 치명적이지는 않지만 중요한 문제가 발생한 경우 사용합니다. 복구가 필요하거나 실패한 작업을 추적할 때 활용됩니다.
- 무저건 Exception이라고, 다 에러로 찍을 필요는 없다. (개발자가 개입할 상황의 경우에 ERROR 레벨로 해주면 좋다.)
#### 6. FATAL
- 시스템 운영을 지속할 수 없을 정도로 심각한 오류가 발생했을 때 사용합니다. 즉시 조치가 필요한 상태를 의미합니다.
- 이 레벨의 로그는 개발자가 찍는게아니라 시스템 자체적으로 찍히는 경우가 많다.

-----------------------------------------------------------------------------------------------------------------------------------------------

# 2. Logback 설정하기

#### Logback, ELK
- 로그설정을 하지않으면 로그는 파일에 남는게 아니라 콘솔에 출력되고 만다. 
- 로그를 파일(혹은 다른 무언가)에 남기기위해서는 스프링부트 기본 로그 프레임워크인 Logback을 통해서 한다. (그외 log4j, log4j2, java.util.logging 등등이 있다.)
- 로그를 파일뿐만 아니라 Elasticsearch(로그DB)에 저장할 수 있는데 그것을 해주는 주체가 바로 Logstash이다.
  - 로그를 바로 Elasticsearch로 보낼 수 있는데 Logstash를 사용하는 이유는 로그를 가공할때는 Logstash가 껴있는게 좋아서이다.
- Elasticsearch에 저장된 로그를 kibana로 시각화한다.


#### @Slf4j란?
- 앞서말한 로그 프레임워크를 감싸는 최상단 interface이다. (logback,log4j, log4j2등은 구현체) 

#### logback 설정파일 (profile별 or 1개)
- resources/logback-<profile>.xml 
- resources/logback.xml // 파일 안에서 springProfile 태그로 profile 분리 가능

#### logback 태그 
- <property>: 변수
- <appender>: 콘솔로 남길지 파일로 남길지 등, 로그형태는 어떠한지 등등을 설정하는 태그 (여러개 동시에 적용가능, 콘솔 or 파일 로그 출력 형태를 다르게 설정한다든가, 어펜더 종류는 여러개이다.) 
- <root>: 로그레벨에 따라 정의된 <appender>를 선택하여 관리할수있다. 즉 info 레벨에서 appender1를 사용하고, debug에서는 appender2를 사용하고.. 이런식
- <conversionRule>: 로그 패턴에서 사용할 커스텀 변환 규칙을 정의하는 태그로 색깔을 바꾸거나 로그 메시지 형식 변경, 데이터 마스킹 (민감정보 숨기기)등 설정할 수 있다.
- <springProfile>: 특정 스프링 프로필(환경)에서만 적용되는 로그 설정을 정의하는 태그이다. 

#### appender 종류 (appender 태그의 class= 속성에 해당)
- Console Appender: 콘솔에 로그를 출력
- File Appender: 지정된 파일에 로그를 기록
- RollingFile Appender: 로그 파일을 자동으로 롤링(분할)하여 기록 (일단위로 로그를 남길 수 있다.)
- AsyncApeender: 비동기적으로 로그를 기록하여 성능을 개선 (정말 성능이 중요한 어플리케이션이라면 생각해볼만한듯)
- SyslogAppender: Syslog 서버(로그 중앙 저장소)로 로그를 전송하는 Appender (요즘엔 ELK스택으로 잘 안씀)
- SoketAppender: 소켓을 통해 로그를 원격서버로 전송
- 이메일,db, 커스텀 어펜더등등 더 많은 어펜더가 존재한다 (대부분 Console, RollingFile 두 개를 가장 많이 사용한다.)

#### RollingFile Appender
- <append>true</append>: 서버 재부팅시 로그파일에 로그를 덮어쓰기(초기화) X, 이어쓰기 O
- <rollingPolicy>: 롤링 정책 설정 (언제, 어떻게 파일을 나눌지 결정)
  - <fileNamePattern>
    - 로그 파일이름 패턴인데, 여기에서 yyyy-MM-dd 이런식으로 시간패턴을 쓰임
    - 이 마지막 시간패턴의 마지막 시간단위가 로그파일이 새롭게 작성되는 단위이다.
    - 예를들어 시간패턴이 dd(날짜)로 끝나면 하루단위, HH(시간)단위로 끝나면 시간단위로 로그저장이 된다.
    - 또한 이름패턴 마지막에 gz확장자를 붙혀주면 로그가 롤링될때 자동으로 gzip으로 압축해준다. 
      - 예시: <fileNamePattern>application.%d{yyyy-MM-dd_HH-mm}.log.gz</fileNamePattern>
  - <maxHistory>: 최대 보관 일수 (예: 30이면 30일치만 보관, 이후 자동 삭제)
  - <totalSizeCap>: 전체 로그 파일의 최대 용량 (예: 20GB 초과 시 오래된 파일부터 삭제)

#### profile별 logback을 설정하는 방법 2가지
1. 파일 분리
- application-dev.yml가 실행될때 -> logback-dev.xml를 읽히게하고
- application-prod.yml가 실행될때 -> logback-prod.xml를 읽히게 해야한다.

```text
- application-dev.yml 내용 logging.config=classpath:logback-dev.xml
- application-prod.yml 내용 logging.config=classpath:logback-prod.xml

src/main/resources/
├── logback-spring.xml (공통)
├── logback-local.xml
├── logback-dev.xml
└── logback-prod.xml
```

2.  한 파일에 <springProfile> 사용 (개인적으로 이게좋은듯)
```xml
<configuration>
    <!-- 공통 설정 -->
    <appender name="console" .../>
    
    <!-- 환경별 설정 -->
    <springProfile name="dev">
        <include resource="logback-dev.xml" />
    </springProfile>
 
    <springProfile name="prod">
        <include resource="logback-prod.xml" />
    </springProfile>
    
    <springProfile name="default">
        <include resource="logback-dev.xml" />
    </springProfile>
</configuration>
```
# 3. 로그 수집

#### Elasticsearch, Logtash 준비하기
- logstash 의존성 추가
- logback.xml 설정 (logstash Appender 추가)
- logstash.conf 작성
- docker-compose-elk.yml 실행 (볼륨 필드에서 logstash.conf 파일만 도커 컨테이너 안에 잘 넣어주면됨)

# 4. 로그 시각화와 활용 

#### kibana로 로그 확인해보기
- 키바나에 접속하여서 "data view"를 생성하여 로그를 볼 수 있다. (view 이름, 조회할 인덱스, 타임스태프 선택 (정렬조건을 타임스탬프로))
- 키바나에서 json 필드로 검색: message: "키워드"
- 키바나에서 json 필드로 검색 and 조건: message: "키워드" and level: "INFO"

#### 대시보드로 로그 데이터 시각화하기
- 키바나에 접속하여서 대시보드를 생성하고 대시보드안에서 visualization 생성한다. (visualization은 여러개 생성가능)
- Visualization은 "data index"와 연동된다. 
- 키바나 시각화는 2가지 방법이 존재한다.
    - 1. 좌측에서 시각화 하고싶은 필드를 드래그앤드랍으로 시각화하기
    - 2. 우측에서 차트종류를 선택한다음 x,y축을 필드설정하여 차트로 보기 (주로 가로축은 Date histogram으로 시간으로 설정.)
- 참고로 하단에 suggestions으로 시각화할수있는 여러 차트 리스트를 보여준다. 클릭해서 다양한 차트로 데이터를 분석할 수 있다.
- 우상단에서 save return을 누르면 방금 완성한 차트를 대시보드에 넣을수있다. 그상태에서 save를 하면 대시보드가 완성된다.

#### 로그레벨을 기준으로 알림 설정하기
- 구현 방법은 밑에처럼 엘라스틱서치 조회로 level별 로그 통계를 조회해서 그 정보를 바탕으로
- 리눅스 서버 cron, 젠킨스, 쿠버네티스의 크론잡등 스케줄링 솔루션을 이용하여 구현하면된다.

```text
예시
- fatal, error  -> 1회라도 발생시 알람
- warn -> 1분간 10회 이상 발생시 알람 + 일단위 리포트
- debug, trace -> 3일치 로그만 보관 
```

```text
url: localhost:9200/application-logs-*/_search

Body (JSON):
{
  "size": 0,             // 이걸 넣어줘야 로그리스트 반환이 안됨(필요한 정보는 aggregations 뿐임)
  "query": {
    "range": {
      "timestamp": {
        "gte": "now-1m", // 기간 설정
        "lt": "now"      // 기간 설정
      }
    }
  },

  "aggs": {
    "log_level_aggregation": {
      "terms": {
        "field": "level.keyword"
      }
    }
  }
}
이렇게 요청하면 반환필드 aggregations에서 다음과 같이 레벨별 로그의 갯수를 확인할 수있다.

"buckets": [
  {
    "key": "TRACE",
    "doc_count": 7204
  },
  {
    "key": "DEBUG",
    "doc_count": 2715
  },
  {
    "key": "INFO",
    "doc_count": 80
  }
]

```


# 5. 부록
#### 표준출력(println) vs 로깅프레임워크 성능차이
- println 100만번 출력: 1600ms 
- SLF4J 100만번 출력: 10ms


#### printstackrace() 그냥출력 vs getStackTrace()로 잡아서 로깅프레임워크로 출력 성능차이
- 위처럼 엄청난 차이는 아니지만 그래도 2배정도 차이난다. 
    - 호출 깊이가 6뎁스로인 경우로 테스트, 뎁스가 1인경우 차이는 없음
    - 깊이가 깊어질수록 성능차이가 나고, 뎁스가 1인경우 차이가 없는 경우는 결국 "스택 추적 정보" 가져오는 비용은 동일하기 때문
- 속도보다 더 중요한 것은 printstackrace()는 로그레벨을 활용할 수 없다. (printstackrace은 표준출력임)