```
npm add @nestjs/typeorm typeorm mysql2
```
ORM, PRISMA

```
import { Module } from '@nestjs/common';

//import { AppController } from './app.controller';

//import { AppService } from './app.service';

import { CustomersModule } from './customers/customers.module';

import { UsersModule } from './users/users.module';

import { TypeOrmModule } from '@nestjs/typeorm';

import { User } from './typeorm';

@Module({

  imports: [

    CustomersModule,

    UsersModule,

    TypeOrmModule.forRoot({

      type: 'mysql',

      host: 'localhost',

      port: 3306,

      username: 'root',

      password: '0523',

      database: 'tutorial_db',

      entities: [User],

      synchronize: true,

    }),

  ],

  controllers: [],

  providers: [],

})

export class AppModule {}
```
app.module에 설정 변경

```
import { Column, Entity, PrimaryGeneratedColumn } from 'typeorm';

  

@Entity()

export class User {

  @PrimaryGeneratedColumn({

    type: 'bigint',

    name: 'user_id',

  })

  id: number;

  

  @Column({

    nullable: false,

    default: '',

  })

  username: string;

  

  @Column({

    name: 'email_address', // 여기는 database에 보이는 이름

    nullable: false, // 공백 설정

    default: '', //  기본 값 설정

  })

  emailAddress: string;

  

  @Column({

    nullable: false,

    default: '',

  })

  password: string;

}
```

database 스키마 설정