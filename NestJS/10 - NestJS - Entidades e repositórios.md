O próximo passo seria criar as entidades e os repositórios.

1. Criar um arquivo da entidade (classe) e dizer as propriedades que ele terá
2. Conectar a entidade ao seu módulo pai, o que cria um repositório
3. Conectar a entidade na raiz da aplicação (appmodule)

```typescript
//Criando a entidade -> recebe os decoradores Entity, Column e PrimaryGeneratedColumn
import { Entity, Column, PrimaryGeneratedColumn } from "typeorm";
@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;
  @Column()
  email: string;
  @Column()
  password: string;
}
```

```typescript
/*Ligando a entidade com o módulo pai. Se usa o método forFeature da classe TypeOrmModule para
registrar a entidade. Isso gera o repositório por debaixo dos panos.
*/
import { Module } from "@nestjs/common";
import { UsersService } from "./users.service";
import { UsersController } from "./users.controller";
import { TypeOrmModule } from "@nestjs/typeorm";
import { User } from "./user.entity";
@Module({
  imports: [TypeOrmModule.forFeature([User])],
  providers: [UsersService],
  controllers: [UsersController],
})
export class UsersModule {}
```

```typescript
//Na raiz (appModule), vai registrar como entidade no array
import { Module } from "@nestjs/common";
import { AppController } from "./app.controller";
import { AppService } from "./app.service";
import { TypeOrmModule } from "@nestjs/typeorm";
import { UsersModule } from "./users/users.module";
import { ReportsModule } from "./reports/reports.module";
import { User } from "./users/user.entity";

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: "sqlite",
      database: "db.sqlite",
      entities: [User],
      synchronize: true,
    }),
    UsersModule,
    ReportsModule,
  ],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

Quando se trabalha com bancos de dados, uma técnica para manter a estutura do banco e o código "semelhantes", se chama de migração (migrations). Basicamente, é uma abordagem 'code-first', que pega uma entidade no código fonte e através de uma ferramenta, vai transcrever o que estiver alí para o banco de dados. Ou seja, ela muda a estrutura do banco baseado no código fonte. Com os decoradores, se passam informações a mais para o banco. Por exemplo:

@PrimaryGeneratedColumn() -> indica que é uma coluna gerada primariamente, ou seja, uma chave primária.

@Column() -> Uma coluna padrão.

Alterando a entidade, o typeorm identifica e realiza uma migração de forma automática. Para ambiente de desenvolvimento, é uma boa técnica, pois tanto o código quanto o banco ficam sincronizados. Em produção já não é recomendado. É melhor escrever as migrações de forma manual.

Existe uma API de métodos que o typeorm cria ao gerar os repositórios:

https://typeorm.io/repository-api

Os métodos são, de forma geral:

1. create -> cria instância, mas não salva no banco
2. save -> salva ou atualiza instância
3. find -> retorna lista de entidades
4. findOne -> retorna o primeiro registro que bate com os critérios
5. remove -> remove do banco

Lembrando -> validação de requisições.

1. instalar o class-validator e o class-transform
2. No main.ts, fazer com que app use pipes globais (app.useGlobalPipes(new ValidationPipe()))
3. Criar os dtos com as validações
4. No método desejado, usar @Body com o tipo do dto criado

O pipe pode receber um objeto de parâmetro:

```typescript
app.useGlobalPipes(
  new ValidationPipe({
    whitelist: true,
  })
);
```

Esse whitelist indica que, se no corpo da requisição passar uma informação a mais fora do especificado, ele é automaticamente retirado.
