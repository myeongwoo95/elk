# shorten-url-for-logging

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
