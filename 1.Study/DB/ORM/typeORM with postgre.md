1. 설치
~~~
npm install --save @nestjs/typeorm typeorm pg
~~~
2. 디비연결
~~~
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'postgres',
      host: 'localhost',
      port: 5432,
      username: 'postgres',
      password: 'your_password',
      database: 'your_db',
      entities: [__dirname + '/**/*.entity{.ts,.js}'],
      synchronize: true, // 개발용. 운영에선 false!
      autoLoadEntities: true, // 모듈 내 entity 자동 인식
    }),
  ],
})
export class AppModule {}
~~~
3. 앤티티 생성
~~~
import { Entity, Column, PrimaryGeneratedColumn, CreateDateColumn, UpdateDateColumn } from 'typeorm';

@Entity()

export class User {

  @PrimaryGeneratedColumn()
  id: number;  

  @Column({ unique: true })
  email: string;

  @Column()
  password: string;

  @Column({ default: false })
  isAdmin: boolean;

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;

}
~~~

# TypeORM 데코레이터 & 기능 정리

NestJS + TypeORM 프로젝트에서 사용할 수 있는 주요 데코레이터와 설정들을 항목별로 정리한 문서입니다.

---

## 1. 엔티티 관련 데코레이터

| 데코레이터 | 설명 |
|------------|------|
| `@Entity()` | 해당 클래스가 DB 테이블이라는 것을 명시 |
| `@PrimaryGeneratedColumn()` | 기본 키 + 자동 증가 (auto increment) |
| `@PrimaryColumn()` | 기본 키 수동 지정 |
| `@Column()` | 일반 컬럼 정의 |
| `@Generated('uuid')` | UUID 자동 생성 필드에 사용 (ex: `@PrimaryColumn`, `@Column` 위에 사용) |

---

## 2. 날짜 관련 데코레이터

| 데코레이터 | 설명 |
|------------|------|
| `@CreateDateColumn()` | 레코드 생성 시 자동으로 현재 시간 입력 |
| `@UpdateDateColumn()` | 레코드 변경 시 자동으로 시간 갱신 |
| `@DeleteDateColumn()` | soft delete 시 삭제 시간을 기록 (softRemove와 함께 사용) |

---

## 3. 관계(Relation) 데코레이터

| 데코레이터                                                    | 설명                                                            |
| -------------------------------------------------------- | ------------------------------------------------------------- |
| `@OneToOne(() => TargetEntity)`                          | 1:1 관계                                                        |
| `@OneToMany(() => TargetEntity, target => target.field)` | 1:N 관계 (여기서 One 쪽)                                            |
| `@ManyToOne(() => TargetEntity, target => target.field)` | N:1 관계 (여기서 Many 쪽)                                           |
| `@ManyToMany(() => TargetEntity)`                        | N:N 관계                                                        |
| `@JoinColumn()`                                          | 외래 키를 지정 (주로 1:1, N:1 관계에서 사용)                                |
| `@JoinTable()`                                           | 다대다 관계 중간 테이블을 생성 (ManyToMany에서 필요)                           |
|                                                          |                                                               |
| **옵션**                                                   | **설명**                                                        |
| type                                                     | 컬럼의 DB 데이터 타입 (e.g. varchar, int, boolean, timestamp, json 등) |
| length                                                   | 문자열 컬럼의 길이                                                    |
| nullable                                                 | null 값 허용 여부                                                  |
| default                                                  | 기본값 지정                                                        |
| unique                                                   | 고유 제약 조건 설정                                                   |
| name                                                     | 실제 DB에 생성될 컬럼명 지정                                             |
|select|기본 조회 쿼리에서 제외할지 여부 (false면 명시적으로만 조회 가능)|

---

4. 앤티티 등록
~~~
import { TypeOrmModule } from '@nestjs/typeorm';
import { User } from './user.entity';
import { UserService } from './user.service';
import { UserController } from './user.controller';

@Module({
  imports: [TypeOrmModule.forFeature([User])], // 반드시 등록!
  providers: [UserService],
  controllers: [UserController],
  exports: [UserService],
})
export class UserModule {}
~~~
5. base 함수 사용 방법
~~~
@Entity()

export class User extends BaseEntity {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;
}
~~~