[[express]]

1. [[get]]: getting data
2. [[post]]: creating data
3. [[put]]: updating data
4. [[delete]]: deleting 
5. patch: 

1. Query Params (req.query.user_id) 
2. Body (req.body.user_id)
3. Path Variables (req.params.user_id)
4. 그리고 위 방식을 혼합해서 사용할 수 있습니다.

#### - GET (조회)

    GET Method는 데이터를 조회하는데 사용됩니다. 보통 3번 방식은 잘 사용하지 않습니다. 참고해서 만드는걸 추천드립니다. :)

#### - POST (생성)

   POST Method 는 데이터를 생성할 때 사용됩니다. 보통 Body 에 데이터를 담아서 전송합니다.

#### - PUT (전체 데이터 수정)

   PUT Method 는 전체 데이터를 수정할 때 사용됩니다. 보통 Body 에서 수정할 데이터를 담아서 전송합니다.

#### - PATCH(단일 데이터 수정)

   PATCH Method 는 단일 데이터를 수정할 때 사용됩니다. 보통 Query Params나 Path Variables와 Body를 혼합에서 잘 사용합니다.

#### - DELETE(삭제)

   DELETE Method 는 데이터를 삭제할 때 사용됩니다. 보통 Query Params나 Path Variables를 잘 사용합니다.

각각의 차이점

1.
2.
3.
4.
5.