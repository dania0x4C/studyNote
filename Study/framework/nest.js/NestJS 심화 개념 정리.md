
**1. Module의 구조와 활용**

• NestJS는 모든 기능을 모듈 단위로 구성함.

• 하나의 모듈에는 controllers, providers, imports, exports가 있음.

• 다른 모듈의 서비스나 설정을 사용하고 싶으면 imports로 불러오고, 사용 대상은 exports에 명시해야 함.

• 기능별로 UserModule, AuthModule처럼 나누고, 공통 기능은 SharedModule 등으로 분리해 관리하는 게 좋음.

  

**2. 동적 모듈 등록 (register(), registerAsync())**

• 설정값(API Key, 옵션 등)을 주입해야 하는 모듈에 사용.

• 예: 외부 API 클라이언트, 메시지 큐, 결제 모듈 등

• registerAsync()를 통해 ConfigService와 연동 가능.

• 설정값이 없는 경우 동작 자체를 막을 수 있어 명확한 의존성 정의에 유리함.

  

**3. ConfigModule + .env 통합 관리**

• .env 파일을 통해 전역 설정값을 통합 관리.

• ConfigModule.forRoot({ isGlobal: true })로 전역 등록하면 어디서든 ConfigService를 통해 설정값 사용 가능.

• 전형적인 설정값: DB 연결 정보, JWT 시크릿, 외부 API 키 등

• joi와 같은 스키마 검증을 통해 안전성도 확보 가능.

  

**4. 사용자 정보 주입 (@CurrentUser)**

• @CurrentUser() 커스텀 데코레이터를 만들어 로그인한 사용자 정보를 컨트롤러에서 바로 사용할 수 있게 함.

• AuthGuard에서 request.user를 설정하고, 데코레이터에서는 이 값을 꺼내서 전달.

• 특정 필드만 추출하는 기능도 함께 구현 가능.

• 코드 가독성과 재사용성이 매우 좋아짐.

  

**5. Interceptor와 전역 예외 필터 구조 (기억해둠)**

• Interceptor: 응답 형식 통일, 로깅, 요청/응답 가공 등에 사용.

• 전역 예외 필터: 모든 예외를 포착하여 일관된 에러 응답 처리.

• app.useGlobalFilters()나 APP_FILTER, APP_INTERCEPTOR로 전역 적용 가능.

  

**6. 트랜잭션 처리 및 DB 의존성 분리**

• Prisma: prisma.$transaction()을 통해 원자적인 트랜잭션 실행 가능.

• 각 레포지토리에 tx(TransactionClient)를 주입하여 트랜잭션 내에서 처리되도록 분리 가능.

• TypeORM: dataSource.transaction(async manager => ...) 방식으로 사용.

• 트랜잭션 제어는 서비스에서, DB 처리 로직은 레포지토리로 분리하는 설계가 유지보수에 좋음.

  

**7. ConfigModule vs registerAsync 차이점**
|**항목**|**ConfigModule 방식**|**registerAsync 방식**|
|---|---|---|
|설정 주입 시점|앱 실행 이후|모듈 초기화 시점|
|적용 범위|전역 (isGlobal: true)|해당 모듈 한정|
|목적|공통 설정값 관리|모듈 전용 설정 주입|
|사용 방식|configService.get('KEY')|useFactory를 통해 설정값 직접 전달|
|추천 사용처|DB, JWT, Redis 등 공통 설정|외부 API, 메일, 클라우드 등 모듈 종속 설정|

