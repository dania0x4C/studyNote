
TypeORM Repository 메서드 전체 정리 (매개변수 포함)

NestJS + TypeORM 환경에서 Repository 객체를 통해 사용 가능한 모든 주요 메서드와 해당 매개변수, 옵션, 예제를 정리한 문서입니다.

---

저장 및 수정 관련

1. save(entity: Entity | Entity[], options?)
- insert 또는 update 자동 판별
- options: transaction, listeners, reload

예시:
repo.save({ id: 1, name: '홍길동' })

2. insert(entity: Entity | Entity[])
- insert 전용, 성능 우수
- 반환값: InsertResult

예시:
repo.insert({ name: 'user' })

3. update(criteria, partialEntity)
- 조건에 맞는 데이터를 수정
- 반환값: UpdateResult

예시:
repo.update({ id: 1 }, { name: '변경됨' })

4. preload(entityLike)
- 기존 Entity를 찾아 병합, 저장은 아님

예시:
repo.preload({ id: 1, name: '업데이트' })

5. merge(mergeIntoEntity, ...sources)
- 여러 객체를 하나로 병합

예시:
repo.merge(user, { name: '합치기' })

---

삭제 관련

6. delete(criteria)
- 완전 삭제
- 반환값: DeleteResult

7. remove(entity | entity[])
- Entity 인스턴스 기반 삭제

8. softDelete(criteria)
- soft delete 수행
- deletedAt 사용
- 반환값: UpdateResult

9. softRemove(entity)
- Entity 인스턴스를 기반으로 soft delete

10. restore(criteria)
- soft delete 복원

---

조회 관련

11. find(options)
- 다수 row 조회
- options: where, order, relations, take, skip, select, withDeleted

12. findOne(options)
- 단일 조회 (복합 옵션 사용 가능)

13. findOneBy(where)
- 단순 조건 단일 조회

14. findOneOrFail(options)
- 단일 조회, 없으면 예외 발생

15. findOneByOrFail(where)
- 단순 조건 + 예외 발생

16. findBy(where)
- 조건 배열로 다수 조회

17. findAndCount(options)
- [데이터, 총 수] 반환

18. count(options)
- 조건에 맞는 총 수 반환

19. exist(options)
- 조건에 해당하는 데이터 존재 여부 확인 (boolean)

---

Entity 생성 관련

20. create(data)
- Entity 인스턴스 생성 (저장 아님)

---

QueryBuilder

21. createQueryBuilder(alias)
- 복잡한 SQL 작성 가능
- where, andWhere, orderBy, leftJoin, take, skip 등과 조합

예시:
repo.createQueryBuilder('user')
  .where('user.name = :name', { name: 'kim' })
  .orderBy('user.createdAt', 'DESC')
  .take(10)
  .getMany()

---

조건 연산자 (FindOperator)

사용 가능한 연산자:
- In, Not, IsNull, Like
- Between, MoreThan, LessThan
- MoreThanOrEqual, LessThanOrEqual

예시:
repo.find({
  where: {
    id: In([1, 2, 3]),
    deletedAt: Not(IsNull()),
    createdAt: Between('2023-01-01', '2023-12-31'),
    age: MoreThan(18),
  }
})

