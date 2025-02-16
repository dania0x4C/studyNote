- ORM(Object-Relational Mapping)은 객체와 관계형 데이터베이스의 데이터를 자동으로 연결해주는 기법 또는 도구이다. 즉, 데이터베이스의 테이블과 객체를 매핑하여 SQL 쿼리 없이도 프로그램 내에서 데이터베이스와 상호 작용할 수 있도록 해줍니다.

## Prisma

#### Connection Pool
Prisma는 데이터베이스와의 효율적인 연결 관리를 위해 자체적인 커넥션 풀링(connection pooling) 메커니즘을 제공한다. 
Prisma Client는 첫 번째 쿼리를 실행할 때 자동으로 데이터베이스에 연결하고 커넥션 풀을 생성한다.
이 커넥션 풀은 데이터베이스 연결을 재사용하여 성능을 향상시키고, 불필요한 연결 생성을 방지할 수 있게 된다.

서버리스 환경에서는 각 함수 호출마다 PrismaClient 인스턴스가 생성되며, 각 인스턴스는 자체 커넥션 풀을 관리하게 되는데 이로 인해 데이터베이스 연결 수가 급격히 증가할 수 있게 된다. 그렇기 때문에 ==외부 커넥션 풀러인 PgBouncer==와 같은 도구를 사용하는 것이 권장된다.

또한, Prisma는 Prisma Accelerate라는 기능을 통해 글로벌 커넥션 풀링과 캐싱 레이어를 제공합니다. 이를 통해 서버리스 배포 시 발생할 수 있는 연결 시간 초과 문제를 완화하고, 전 세계 사용자에게 빠른 응답을 제공합니다.? 이건 뭔지 나중에 더 봐야할 듯

#### Migration
데이터베이스 스키마 변경 관리를 위해 **Prisma Migrate**라는 기능을 제공

Prisma는 데이터베이스 스키마 변경을 효율적으로 관리하기 위해 **Prisma Migrate** 도구를 제공합니다. 이를 통해 데이터베이스 스키마와 Prisma 스키마를 동기화하고, 마이그레이션 파일을 생성하여 버전 관리를 할 수 있습니다.

**Prisma Migrate의 주요 기능:**

1. **마이그레이션 생성 및 적용:**
    
    - `npx prisma migrate dev --name [migration_name]` 명령어를 사용하여 새로운 마이그레이션을 생성하고 개발 환경에 적용할 수 있습니다.
        
    - 프로덕션 환경에서는 `npx prisma migrate deploy` 명령어를 사용하여 모든 대기 중인 마이그레이션을 적용합니다.
        
2. **마이그레이션 상태 확인:**
    
    - `npx prisma migrate status` 명령어를 통해 현재 마이그레이션 상태를 확인하고, 적용되지 않은 마이그레이션이 있는지 확인할 수 있습니다.
        
3. **마이그레이션 리셋:**
    
    - `npx prisma migrate reset` 명령어를 사용하여 데이터베이스를 초기화하고 모든 마이그레이션을 다시 적용할 수 있습니다. 이 명령어는 개발 환경에서 주로 사용되며, 데이터베이스의 모든 데이터를 삭제하므로 주의가 필요합니다.
        
4. **마이그레이션 파일 관리:**
    
    - 각 마이그레이션은 `prisma/migrations` 디렉토리에 저장되며, SQL 파일 형식으로 생성됩니다. 이를 통해 마이그레이션 내역을 추적하고 버전 관리를 할 수 있습니다.
        

**주의사항:**

- Prisma Migrate는 데이터베이스 스키마 변경을 관리하는 도구로, 데이터 마이그레이션(예: 데이터 변환 또는 이전)을 직접 지원하지 않습니다. 데이터 마이그레이션이 필요한 경우, 별도의 스크립트를 작성하거나 수동으로 처리해야 합니다.
    
- 마이그레이션을 적용하기 전에 데이터베이스를 백업하여 데이터 손실을 방지하는 것이 좋습니다.
    

Prisma Migrate를 활용하면 데이터베이스 스키마 변경을 체계적으로 관리하고, 개발 및 배포 프로세스를 효율적으로 진행할 수 있습니다.


### prisma 장단점

### 장점

1. **자동화된 마이그레이션 관리**: Prisma Migrate는 데이터베이스 스키마 변경을 자동으로 관리하고, SQL 쿼리를 수동으로 작성하지 않아도 되기 때문에 개발 속도가 빨라집니다.
2. **일관성 유지**: 모든 마이그레이션이 코드에 기록되므로 팀 전체에서 일관된 데이터베이스 상태를 유지할 수 있습니다. 버전 관리 시스템을 통해 스키마 변경 내역을 추적하고, 협업에 유리합니다.
3. **쉬운 사용법**: 명령어 기반 인터페이스가 직관적이며, 개발 환경에서 쉽게 마이그레이션을 생성 및 실행할 수 있어 초보자도 쉽게 사용할 수 있습니다.
4. **개발 및 프로덕션 환경 분리**: 개발 환경과 프로덕션 환경에 맞게 마이그레이션을 관리할 수 있습니다. `dev`와 `deploy` 명령어를 분리하여 안전한 배포를 지원합니다.
5. **Schema-First 접근**: 데이터베이스 모델링이 직관적이며, `schema.prisma` 파일에서 쉽게 모델을 정의하고 관리할 수 있습니다.
6. 쉽기 때문에 더 복잡한 수준의 api를 관리 할 수 있다.
7. real-time 데이터 처리가 가능해진다.

### 단점

1. **데이터 마이그레이션 미지원**: Prisma Migrate는 데이터베이스 스키마 변경을 위한 도구로, 데이터 자체를 이전하거나 변환하는 작업은 직접 스크립트를 작성해야 합니다.
2. **제한된 데이터베이스 지원**: Prisma는 일부 데이터베이스(예: MySQL, PostgreSQL, SQLite, SQL Server)만을 지원하며, 모든 데이터베이스에 최적화된 솔루션은 아닙니다.
3. **서버리스 환경에서 연결 문제**: 서버리스 환경에서는 데이터베이스 연결 수가 급증할 수 있으며, 별도의 커넥션 풀링 도구(PgBouncer 등)가 필요할 수 있습니다.
4. **롤백 제한**: Prisma Migrate는 기본적으로 마이그레이션 롤백 기능을 제공하지 않아, 롤백이 필요할 경우 수동으로 관리해야 합니다. 이로 인해 복잡한 마이그레이션에서 관리가 어려울 수 있습니다.
5. **대규모 변경 관리 어려움**: 복잡하거나 대규모의 스키마 변경이 필요한 경우, Prisma Migrate가 제한적으로 느껴질 수 있으며, SQL 기반의 세밀한 조정이 필요할 수 있습니다.

요약하면 
장점은 편리하고 협업하기 편하고 버전 관리가 쉽다
단점은 serverless에서 문제가 있고 대규모 스키마 변경에서 제한적이며 모든 DB를 지원하는 것이 아니다.


#### N+1 문제

연관 관계에서 발생하는 이슈로 연관 관계가 설정된 엔티티를 조회할 경우에 조회된 데이터 갯수(n) 만큼 연관관계의 조회 쿼리가 추가로 발생하여 데이터를 읽어오게 된다. 즉, 1번의 쿼리를 날렸을 때 의도하지 않은 N번의 쿼리가 추가적으로 실행되는 것이다. 이를 N+1 문제라고 한다.



### **쿼리(**[**Query**](https://www.elancer.co.kr/blog/view?seq=279)**) 성능 분석**

성능 최적화의 첫 단계는 쿼리 성능을 정확하게 분석하는 것입니다. **Prisma는 쿼리 로그와 성능 측정 도구를 제공하여, 실행된 쿼리의 성능을 모니터링하고 분석할 수 있도록 돕습니다.** 예를 들어, Prisma Studio를 사용하면 실행된 쿼리와 그 성능을 시각적으로 확인할 수 있습니다. 이러한 툴을 활용해 어떤 쿼리가 성능 저하의 원인이 되는지 파악할 수 있습니다.

### **쿼리(**[**Query**](https://www.elancer.co.kr/blog/view?seq=279)**) 최적화 전략** 

**쿼리(**[**Query**](https://www.elancer.co.kr/blog/view?seq=279)**) 성능 분석 결과를 바탕으로, 특정 쿼리의 성능을 개선할 전략을 수립할 수 있습니다.** 예를 들어, 너무 많은 [데이터](https://www.elancer.co.kr/blog/view?seq=241)를 한 번에 불러오는 N+1(N + 1문제란 1번의 쿼리를 날렸을 때 의도하지 않은 N번의 쿼리가 추가적으로 실행되는 것을 의미) 문제를 해결하기 위해 적절한 쿼리 결합과 [데이터](https://www.elancer.co.kr/blog/view?seq=241) 로딩 전략을 적용할 수 있습니다. 

또한, 색인 추가, 쿼리 로직의 수정, 캐싱 전략의 도입 등 다양한 방법으로 쿼리 성능을 최적화할 수 있습니다. 이러한 **성능 최적화 과정을 통해 애플리케이션의** [**데이터**](https://www.elancer.co.kr/blog/view?seq=241) **처리 속도는 더욱 빨라지고, 결국 사용자에게 제공되는** [**서비스**](https://www.elancer.co.kr/blog/view?seq=230)**의 질은 크게 향상**됩니다.

### `findUnique`와 `findMany` 정리된 표

| 옵션                 | `findUnique`     | `findMany`     |
| ------------------ | ---------------- | -------------- |
| `where`            | 고유 필드 조건         | 조건 필터          |
| `include`          | 관계된 데이터 포함       | 관계된 데이터 포함     |
| `select`           | 특정 필드 선택         | 특정 필드 선택       |
| `rejectOnNotFound` | 데이터가 없을 경우 예외 발생 | 사용 불가          |
| `orderBy`          | 사용 불가            | 데이터 정렬         |
| `skip`             | 사용 불가            | 지정된 수만큼 건너뛰기   |
| `take`             | 사용 불가            | 반환할 레코드 수 제한   |
| `cursor`           | 사용 불가            | 특정 레코드부터 검색 시작 |
| `distinct`         | 사용 불가            | 중복 제거          |
### Prisma에서 사용 가능한 변수형

|데이터 타입|설명|
|---|---|
|`Int`|정수형|
|`String`|문자열형|
|`Boolean`|불리언형 (true/false)|
|`Float`|부동 소수점 숫자형|
|`DateTime`|날짜 및 시간형|
|`Json`|JSON 형식의 데이터|
|`Bytes`|바이너리 데이터|
|`Decimal`|소수점이 있는 고정 소수점 숫자형|
|`BigInt`|64비트 정수형|

### Prisma 고급 타입 및 옵션

| 옵션 및 속성                    | 설명                       | 예시                                          |
| -------------------------- | ------------------------ | ------------------------------------------- |
| `?`                        | 필드가 선택 사항임을 나타냄          | `name String?`                              |
| `@id`                      | 기본 키로 설정                 | `id Int @id`                                |
| `@default`                 | 기본값 설정                   | `createdAt DateTime @default(now())`        |
| `@unique`                  | 고유 제약 조건 설정              | `email String @unique`                      |
| `@map`                     | 데이터베이스에서의 사용자 정의 필드 이름   | `fullName String @map("full_name")`         |
| `@relation`                | 관계 설정                    | `author User @relation(fields: [authorId])` |
| `@updatedAt`               | 업데이트 시 자동으로 갱신되는 필드      | `updatedAt DateTime @updatedAt`             |
| `@index`                   | 인덱스 설정                   | `@@index([title, createdAt])`               |
| `@unique`                  | 특정 필드 또는 필드 조합에 고유 제약 조건 | `@@unique([firstName, lastName])`           |
| `default(uuid())`          | 고유한 UUID 생성              | `id String @id @default(uuid())`            |
| `default(cuid())`          | 고유한 CUID 생성              | `id String @id @default(cuid())`            |
| `default(now())`           | 현재 날짜와 시간 설정             | `createdAt DateTime @default(now())`        |
| `default(autoincrement())` | 자동 증가 값 설정               | `id Int @id @default(autoincrement())`      |

### Prisma 모델 속성

|속성|설명|예시|
|---|---|---|
|`@@map`|테이블 이름을 커스텀 설정|`@@map("users_table")`|
|`@@unique`|다중 필드 고유 제약 조건 설정|`@@unique([email, username])`|
|`@@index`|인덱스 설정|`@@index([name, createdAt])`|

### 엔티티 및 기능 상세 설명

|범주|기능 및 속성|설명|예시|
|---|---|---|---|
|**엔티티**|`enum`|열거형을 정의하여 특정 필드가 미리 정의된 값만 가질 수 있도록 함|`enum UserRole { ADMIN USER GUEST }`|
||`model`|데이터베이스의 테이블을 정의하는 기본 엔티티|`model User { id Int @id @default(autoincrement()) }`|
||`generator`|Prisma 클라이언트를 생성하기 위한 설정|`generator client { provider = "prisma-client-js" }`|
||`datasource`|데이터베이스 연결 설정|`datasource db { provider = "mysql" url = env("DATABASE_URL") }`|

### 데이터베이스 마이그레이션 옵션 상세 설명

|범주|기능 및 속성|설명|예시|
|---|---|---|---|
|**데이터베이스 마이그레이션 옵션**|`@@map`|모델 또는 필드의 데이터베이스 이름을 변경하여 매핑|`@@map("users_table")`|
||`@db.*`|데이터베이스 특정 속성을 설정할 때 사용|`name String @db.VarChar(255)`|

### 특정 필드 속성 상세 설명

|범주|기능 및 속성|설명|예시|
|---|---|---|---|
|**특정 필드 속성**|`@default(expr)`|기본값으로 특정 표현식을 사용|`createdAt DateTime @default(now())`|
||`@assertEqual`|필드의 값이 데이터베이스에서 특정 값과 같은지 확인|사용 예시 없음 (잘 사용되지 않음)|
||`@assertTrue`|필드의 값이 `true`인지 확인|사용 예시 없음 (잘 사용되지 않음)|
||`@assertFalse`|필드의 값이 `false`인지 확인|사용 예시 없음 (잘 사용되지 않음)|
```
// 데이터베이스 연결 설정
datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}

// Prisma 클라이언트 생성기 설정
generator client {
  provider = "prisma-client-js"
}

// 열거형 정의
enum UserRole {
  ADMIN
  USER
  GUEST
}

// 모델 정의
model User {
  id        Int       @id @default(autoincrement()) // 기본 키와 자동 증가 설정
  email     String    @unique // 고유 제약 조건 설정
  name      String?
  role      UserRole  @default(USER) // 열거형 필드와 기본값 설정
  profile   Profile?
  posts     Post[]
  createdAt DateTime  @default(now()) // 현재 시간 기본값
  updatedAt DateTime  @updatedAt // 업데이트 시 자동 갱신
  isActive  Boolean   @default(true)

  @@map("users_table") // 테이블 이름 변경
}

model Profile {
  id        Int    @id @default(autoincrement())
  bio       String?
  userId    Int    @unique
  user      User   @relation(fields: [userId], references: [id])

  @@map("user_profiles") // 테이블 이름 변경
}

model Post {
  id        Int       @id @default(autoincrement())
  title     String    @db.VarChar(255) // 데이터베이스 특정 속성 사용
  content   String?
  published Boolean   @default(false)
  authorId  Int
  author    User      @relation(fields: [authorId], references: [id])
  createdAt DateTime  @default(now())

  @@index([title, createdAt]) // 복합 인덱스 설정
  @@map("posts_table") // 테이블 이름 변경
}
```

```
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

async function main() {
  // findUnique 예제
  const uniqueUser = await prisma.user.findUnique({
    where: {
      id: 1, // 고유 ID로 검색
    },
    include: {
      posts: true, // 사용자와 연관된 게시물 포함
      profile: true, // 사용자 프로필 포함
    },
    select: {
      id: true,
      email: true,
      name: true,
      role: true,
    },
    rejectOnNotFound: true, // 데이터가 없을 경우 예외 발생
  });
  console.log('Unique User:', uniqueUser);

  // findMany 예제
  const manyPosts = await prisma.post.findMany({
    where: {
      published: true, // 게시된 포스트만 검색
    },
    include: {
      author: {
        select: {
          id: true,
          name: true,
          email: true,
        },
      },
    },
    orderBy: {
      createdAt: 'desc', // 생성일 기준 내림차순 정렬
    },
    take: 5, // 상위 5개만 반환
    skip: 2, // 첫 2개 레코드는 건너뜀
    cursor: {
      id: 3, // 특정 ID를 기준으로 시작
    },
    distinct: ['title'], // 중복된 타이틀 제거
  });
  console.log('Many Posts:', manyPosts);
}

main()
  .catch((e) => {
    console.error(e);
    process.exit(1);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });
```
