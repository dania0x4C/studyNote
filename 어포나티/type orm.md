~~~
npm install typeorm pg
~~~

엔티티 만드는 방법

~~~
import { Entity, PrimaryGeneratedColumn, Column, CreateDateColumn } from "typeorm";

@Entity("users")  // 테이블 이름
export class User {
  @PrimaryGeneratedColumn() // Auto Increment (SERIAL)
  id: number;

  @Column({ type: "varchar", length: 100, unique: true })
  username: string;

  @Column({ type: "text", nullable: true })
  bio: string;

  @Column({ type: "boolean", default: true })
  isActive: boolean;

  @CreateDateColumn({ type: "timestamp" })  // 자동 생성 날짜
  createdAt: Date;
}
~~~

~~~
import { Repository } from "typeorm";
import { AppDataSource } from "../data-source";
import { User } from "../entities/User";

export class UserRepository {
  private userRepo: Repository<User>;

  constructor() {
    this.userRepo = AppDataSource.getRepository(User);
  }

  // ✅ 모든 유저 가져오기
  async findAll() {
    return this.userRepo.find();
  }

  // ✅ 특정 유저 찾기
  async findById(id: number) {
    return this.userRepo.findOne({ where: { id } });
  }

  // ✅ 새로운 유저 추가
  async createUser(username: string, bio: string) {
    const newUser = this.userRepo.create({ username, bio });
    return this.userRepo.save(newUser);
  }

  // ✅ 유저 정보 업데이트
  async updateUser(id: number, bio: string) {
    return this.userRepo.update(id, { bio });
  }

  // ✅ 유저 삭제
  async deleteUser(id: number) {
    return this.userRepo.delete(id);
  }
}
~~~
