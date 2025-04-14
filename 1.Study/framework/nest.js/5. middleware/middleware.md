정의
- middleware는 request-reponse 주기의 중간에서 실행되며, request을 처리하거나 response을 생성하기 전에 특정 작업을 수행할 수 있습니다.
- 미들웨어는 모듈 별로 적용할 수 있어, 특정 모듈이나 라우트에 한정하여 동작하게 할 수 있습니다.

사용방법
## multiple middleware
```
customer.apply(cors(), helmet(), logger).forRoutes(ControllerName);
```
### 미들웨어 설명
**CORS (Cross-Origin Resource Sharing)**
    - `cors()`: 이 미들웨어는 **다른 출처의 웹 페이지에서 리소스를 요청**할 수 있게 해주는 보안 기능을 제공합니다. 이를 통해 브라우저가 크로스 도메인 요청을 안전하게 처리할 수 있게 합니다.
    - `cors` 미들웨어는 일반적으로 HTTP 서버에서 CORS 설정을 관리하는 데 사용됩니다.
    
**Helmet**
    - `helmet()`: 이 미들웨어는 **HTTP 헤더를 설정**하여 몇 가지 잘 알려진 웹 취약점으로부터 애플리케이션을 보호합니다.
    - `helmet`은 보안 강화를 위한 여러 미들웨어의 집합으로, XSS 공격, 클릭재킹, MIME 타입 스니핑 등을 방지하는 데 도움이 됩니다.
    
 **Logger**
    - `logger`: **커스텀 로깅 미들웨어**입니다. 요청과 응답을 로깅하거나 애플리케이션의 활동을 기록하는 데 사용됩니다.
    - 로깅 미들웨어는 애플리케이션의 디버깅과 모니터링에 유용합니다.
    
```
# middleware

import { Injectable, NestMiddleware } from '@nestjs/common';

import { NextFunction, Request, Response } from 'express';

  

@Injectable()

export class ValidateCustomerMiddleWare implements NestMiddleware {

  use(req: Request, res: Response, next: NextFunction) {

    console.log('ValidateCustomerMiddleware');

    next();

  }

}
```
1. middleware 정의하기

```
# customers.module
@Module({
  controllers: [CustomersController],
  providers: [CustomersService],
})

// 이 방식으로 module에 Middleware적용
export class CustomersModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(ValidateCustomerMiddleWare).forRoutes(
      {
        path: 'customers/search/:id',
        method: RequestMethod.GET,
      },
  // {},// 이런 방식 가능
    );
  }
}

// 다른 방식으로 middleware 사용
export class CustomersModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(ValidateCustomerMiddleWare)
      .exclude({
        path: 'customers/create',
        method: RequestMethod.POST,
      })
      .forRoutes(
        CustomersController,
      );
  }
}

// middleware를 여러개 사용하는 방법
export class CustomersModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(ValidateCustomerMiddleWare, ValidateCustomerAccountMiddleware)
      .forRoutes(CustomersController);
  }
}

// 그냥 함수 자체를 넣는 것도 가능은 하다
```

1. configure로 사용 할 middleware 받기
2. forRoutes는 경로를 설정하는 method
3. 추가로 middleware를 적용할 경로를 적어주는 것이 가능
4. 모든 controller를 포함시키고 exclude로 제외하는 것이 가능

## global middleware

```
# main.ts

const app = await NestFactory.create(AppModule);
app.use(logger);
await app.listen(3000);
```
main.ts에서 바로 app.use(middlewareLogger)사용하면 됨