- 특정 기능을 동적으로 활성화 비활성화 하기 위해 사용되는 조건부 코드 실행 매커니즘
- 특정 기능을 특정 인원에만 접근 할 수 있게 하여 운영하는 방법
- 가장 중요한 포인트는 기능을 자유자재로 활성화, 비활성화 할 수 있다는 것
- 사용자 특징에 따라 기능을 분리하여 제공할 수 있다.
- 중요한 것은 기능들의 사용성을 높이기 위한 역할 보다는 데이터 관리의 관점에서 생각해야 한다.
- ex) 유튜브 pip 모드

### 장점
- web, app의 설정 값을 수정하지 않아도 된다.
- 검증이 안 된 기능을 제공해야할 때 문제 발생시 원래 기능으로 코드 변경 없이 빠르게 rollback이 가능하다.

오픈 소스
- flagd 

1. **특정 기간 동안 unit 가격 조정** → `TEMPORARY_UNIT_PRICE_OVERRIDE`
    
2. **특정 횟수 동안 unit 가격 조정** → `LIMITED_USAGE_UNIT_PRICING`
    
3. **이용권 구매 시 특정 기간 동안 할인율 조정** → `TIME_BASED_VOUCHER_DISCOUNT`

4. **특정 기간 내 회원가입 시 서비스 할인 적용** → `NEW_MEMBER_PROMOTION`
    
5. **무료 체험 티켓 제공** → `FREE_TRIAL_TICKETS`

6. **특정 크기 이상의 이미지 분할 시 추가 티켓 필요** → `LARGE_IMAGE_EXTRA_TICKET_COST`

7. **시간대별 가격 할인 (Off-Peak Pricing)** → `TIME_BASED_DISCOUNT`

---

~~~ postgre -> typeorm

1. 만든 시간 - Date
2. 업데이트 시간 - Date
3. 삭제된 시간 - Date
4. 정책 시작 시간 - Date
5. 정책 중지 시간 - Date
6. 정책의 우선순위 - Number
7. 변경이된 정책 정보 - {} - generic
8. 적용 대상 정보 - (회원 가입 기간, 구매한 일자 기반, 모든 사용자(default))
9. 적용 이유 - string - ex) 최초 가입 프로모션
10. 적용 상태 - enum(Active, Inactive) 
~~~

제일 중요한 것은 flag라는 것의 의미는 기존의 사용성에 대한 고민을 하기 보다는 이 데이터의 관리의 측면에서의 고민이 가장 중요하다
추가로 데이터가 어떤 값이 들어오든 해당하는 반환 값만 반환 해주면 된다는 것을 생각하자

~~~
기능
1. 정책을 등록하는 기능 X
2. 정책을 변경하는 기능 X - 각 상황마다 변경하는게 좋은가? 필요하면 생성하면 됨
3. 정책을 삭제하는 기능 X - 안 보이게는 할 수 있긴 함
4. 정책 적용 온오프 기능 X
5. 제일 중요한거 실제로 값을 변경할 수 있는걸 파악해서 각각 적용 시켜주는 로직 작성하기
~~~

---

## 우선 순위를 적용하기 위한 처리
1. 우선 기본적으로 서비스에서 우선 순위가 아닌 일반 적용된 정책이 보여질건데
2. 다음과 같이 생각할 수 있다, 새롭게 들어온 데이터의 우선순위가 높아서 이전의 값이 아닌 새로운 값을 내보내야 할때 where 조건 문을 통해 걸러내고 페이지네이션을 적용 시키면 된다.
3. 할인율을 자동으로 제일 큰 값으로 (사용자 측면에서 혜택이 가장 많은걸로?), 가장 적은 티켓을 사용하는 방향으로
4. 가장 늦게 끝나는거
--- 
이곳에는 나중에 모듈이 추가 되어도 사용 가능하게 하는 것이 목표

변경 가능한 데이터
1. 분할된 이미지 마다의 차감되는 티켓의 수
2. 보너스 티켓의 수
3. 티켓 구매 가격

- 결국 산술이 가능한 무언가만 있을면 될듯 여기 수준에서는
- 나중에 모듈이 추가 될 수 있게도 하는 방향으로 가면 재사용성 측면과 지금 당장의 이용성 측면에서는 완성

변경을 요구할 수 있는 구분자
1. 특정 기간
	- 특정 기간에 티켓 구매
	- 특정 기간에 회원 가입
2. 모든 이용자
3. 관리자가 지정한 특정 이용자(이벤트 당첨자)// 임의로 지정 가능하게 해도 될지도 특정 이벤트에 해당하는 id 같은 방식으로
4. 나이 - 받는 데이터가 없음
5. 권한
6. 특정 도메인 이용자
7. 

- 변경을 요구할 수 있는 특정 사용자를 추가하거나 지정할 수 있는 (있는 데이터 내에서 제작을 해야함)
- 여기 부분의 생각이 더 필요할듯
- 시간, 티켓 종류, 유저 종류, 분할된 이미지의 수

---
이렇게 제작을 이어가는 도중 결국 -> 수치적인 변환에 대한 것은 그냥 하나의 모듈로 만들어도 다를게 없다고 판단

rollback

```
import { Injectable } from "@nestjs/common";

import { FlagRepository } from "./flag.repository";

import { FlagModuleType, FlagModuleData } from "src/common/database/entity/flag.entity";

  

@Injectable()

export class FlagTargetService {

constructor(private readonly flagRepository: FlagRepository) {}

  

async processModule(moduleData: FlagModuleData) {

const { type } = moduleData;

  

switch (type) {

case FlagModuleType.USER_ACTIVATION:

return {

type,

message: '사용자 활성화 적용됨'

};

  

case FlagModuleType.USER_DEACTIVATION:

return {

type,

message: '사용자 비활성화 적용됨'

};

  

default:

throw new Error(`지원하지 않는 모듈 타입입니다: ${type}`);

}

}

  

async processModules(modules: FlagModuleData[]) {

const results = [];

for (const moduleData of modules) {

try {

const result = await this.processModule(moduleData);

results.push(result);

} catch (error) {

results.push({

type: moduleData.type,

error: error.message

});

}

}

return results;

}

}

```

```
import { Injectable } from "@nestjs/common";

import { FlagRepository } from "./flag.repository";

import { FlagModuleType, FlagModuleData } from "src/common/database/entity/flag.entity";

  

@Injectable()

export class FlagDataService {

constructor(private readonly flagRepository: FlagRepository) {}

  

private async adjustValue(value: number, changeValue: number) {

if (changeValue === undefined) {

throw new Error("changeValue 값이 필요합니다.");

}

if (Math.abs(changeValue) < 1) {

// 퍼센트(%)라면 원본 + (원본 * changeValue)

return Math.round(value + Math.abs(value * changeValue));

} else {

// 특정 값이라면 원본 + 특정 값

return Math.round(value + changeValue);

}

}

  

async processModule(moduleData: FlagModuleData) {

const { type, value, changeValue } = moduleData;

  

switch (type) {

case FlagModuleType.TICKET_BONUS:

return {

type,

originalValue: value,

newValue: await this.adjustValue(value, changeValue),

message: '티켓 보너스 적용됨'

};

  

case FlagModuleType.TICKET_DEDUCTION:

return {

type,

originalValue: value,

newValue: await this.adjustValue(value, -changeValue), // 차감이므로 음수로 변환

message: '티켓 차감 적용됨'

};

  

case FlagModuleType.TICKET_PRICE:

return {

type,

originalValue: value,

newValue: await this.adjustValue(value, changeValue),

message: '티켓 가격 변경됨'

};

  

default:

throw new Error(`지원하지 않는 모듈 타입입니다: ${type}`);

}

}

  

async processModules(modules: FlagModuleData[]) {

const results = [];

for (const moduleData of modules) {

try {

const result = await this.processModule(moduleData);

results.push(result);

} catch (error) {

results.push({

type: moduleData.type,

error: error.message

});

}

}

return results;

}

}

```

--- 
롤백하고 다시
return value는 그냥 의미가 없고
시간에 따른 적용하는 관리 방식이나 우선순위에 따른 값이 어떻게 나올지에 대한 로직이 필요함 기존의 함수를 이용하고



---

다음에 할일 
1. 값이 잘 나오나 확인
2. 시간이 지나면 비활성화, 활성화 되게 만들기
3. 시간이 아니더라도 value의 내용이 조절 가능하게 만들기 위한 것으로// 빼고 그냥 시간으로 제어하게 만들자 그게 맞다


---
그냥 관리 측면에서 active, noavtive 두기로 직관적이니까

---
이제 마지막으로 할일은 트랜잭션 관리, 동시에 두가지 이벤트를 등록해두었을때 어떻게 관리할것인가 다음에 사용될 아이디를 저장하게 만들어서 끝났을때 끝난 플레그를 요청해도 다음에 사용될 아이디를 반환하게 하면 될듯
여러개를 등록하기 보다는 이전의 값만 넣어줘서 체인으로 걸어주면 될듯 새롭게 만들때 이전 값을 넣기 보다는 새로 만들때 원래 있는 값에 데이터를 넣어주는 형태로 만들기 그리고는 원래 가지고 있던 아이디를 새롭게 적용될 flag아이디를 바꿔주기 이렇게 하면 출력 데이터를 바꾸지 않고 유지 가능

### 2. 만료 타임스탬프 활용하기

TypeORM에서는 쿼리 빌더를 사용하여 만료 타임스탬프를 확인하는 조건을 쉽게 추가할 수 있습니다. 예를 들어, 만료 날짜가 오늘 날짜보다 큰 데이터만 조회하려면 다음과 같이 작성할 수 있습니다.



---


1. 컨트롤러 보면서 내가 원하는 방향으로 만들어졌는지 전부 확인 필요
2. 부모의 키 받는거 말고 자동으로 그룹내에서 검색해서 연결해두게 하는거 필요


이제 값 조회랑, 값 생성 부분 고치면 끝

---


1. 정렬 기본 값으로 해두었으면 값 지정 지우기
2. 업데이트 굳이?
3. 그룹의 역할이 결국 key의 역할인듯 -> unique 뺴자

생성 관리
1. 하나의 키에 플래그는 3개만 가능

시간 관리
2. 사용중인 플래그를 기준으로 새롭게 등록이 된다면 이전의 플래그 값은 사라짐
3. 예약은 3개 까지 가능하며 기존의 값으로 변동 되는 것은 아님 시간이 겹치게 된다면 적용이 되는 동시에 기존의 플래그 값은 사라지는 형태임

그럼 이게 시간이 안 지나도 새로운 값을 사용하게 된다면 삭제를 할거니까 이거 사용에서 처리
만들때는 그냥 생성


값을 보여줄때
1. 우선 키가 active 상태인지 확인
2. 키의 정보를 불러옴(최대 3개)
3. 키의 값 중 
- 존재하는 키중 생성된 시간이 가장 이른 value값을 보여줌


생성할때 마지막 생성된 값이 기준이고 마지막 값이 앞에 만든 값의 기간에 사이에 있다면 신규 생성을 하면서 기존의




생성

1. 같은 키값을 가진 값 중에서 마지막에 생성된 값과 그 앞의 값을 조회
2. 마지막에 생성된 값이
	1. 적용 시간이라면 추가로 생성해서 예약이 가능하고(새롭게 생성된 값이 예약 값)
	2. 적용 시간이 아니라면 이 값이 예약 된 값(새롭게 생성이 안 됨)
3. 일단 값을 생성할때는 이전의 값과 겹쳐도 상관이 없음 왜냐하면 값은 어처피 마지막에 생성된걸 기준으로  보여줄 것이기 때문

조회

1. 마지막 값과 이전의 값을 조회
	1. 만약 마지막 값이 현재 사용 범위에 있으면 그 value를 반환시켜주고
	2. 만약 마지막 값이 현재 사용 범위가 아니면 앞의 값의 사용 범위를 확인해서 적용 가능 범위 이면 value를 반환
	3. 만약 둘다 범위가 아니라면 null 값을 반환

키의 예약값 조회

1. 키의 예약 값을 조회할때 값은 마지막을 기준으로 사용 가능 날짜이면 예약이 없고 그 값이 사용 가능 값
2. 만약 사용 불가능 날짜이면 마지막 값이 예약
	1. 앞의 값이 사용 가능 날짜이면 사용 가능 값
	2. 아니라면 사용 가능 값은 null
3. \


---
1. 전체 조회에서 플래그들의 값들을 전체적으로 보여주는 로직에서 수정
	1. 조회에 있어서 마지막에 생성된 2개의 값만 가져올 것 
		1. 마지막에 생성된 값이 현재 적용 가능한 시간
key: { 
	current:{// 마지막의 생성된 값
		* - value: 플래그 값
		* - startDate: 적용 시작일
		* - endDate: 적용 종료일
		* - type: 타입
	},
	reserve: {// null
		value: 플래그 값
		type: 타입
		startDate: 적용 예정일
	}
}

* - isCurrent: 현재 적용 가능 여부}
		1. 마지막에 생성된 값이 현재 적용 불가능한 시간
			1. 마지막 -1 의 값의 시간이 현재 적용 가능한 시간
				- 마지막 -1 도 적용 가능
	key: { 
	current:{// 마지막-1의 생성된 값
		* - value: 플래그 값
		* - startDate: 적용 시작일
		* - endDate: 적용 종료일
		* - type: 타입
	},
	reserve: {// 마지막의 생성된 값
		value: 플래그 값
		type: 타입
		startDate: 적용 예정일
	}
}
			2. 마지막-1의 생성된 값이 현재 적용 불가능한 시간
			key: { 
	current:{//null
		* - value: 플래그 값
		* - startDate: 적용 시작일
		* - endDate: 적용 종료일
		* - type: 타입
	},
	reserve: {// 마지막에 생성 된 값
		value: 플래그 값
		type: 타입
		startDate: 적용 예정일
	}
}