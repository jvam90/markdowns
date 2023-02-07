NestJS por padrão funciona bem com duas opções:

1. ORM - mapeador entidade-objeto, que trabalha com o banco de dados por meio de classes e código-fonte
2. Mongoose - biblioteca que faz a conexão entre MongoDB e a aplicação

No primeiro momento, a configuração vai ser TypeORM + sqlite, passando para TypeORM + postgres.

Para começar, é preciso usar as seguintes bibliotecas:

```bash
npm install @nestjs/typeorm typeorm sqlite3
```

1. @nestjs/typeorm - integração do nestjs com typeorm
2. typeorm - biblioteca padrão do typeorm
3. sqlite3 - biblioteca padrão do sqlite

Uma primeira organização do projeto seria:

1. AppModule -> módulo principal que vai inicializar e conter toda a aplicação
2. Conexão com o banco de dados dentro do AppModule: criando a conexão no AppModule, todas e quaisquer dependências dele terão acesso à conexão
3. UsersMódule -> módulo de usuários com uma entidade Users e uma classe UsersRepository
4. UsersMódule -> módulo de relatórios com uma entidade Reports e uma classe ReportsRepository

No entanto, quando se usa uma biblioteca como o typeorm, ao gerar as classes, os repositórios são gerados por detrás dos panos.

Como foi dito, a conexão de dados vai para dentro do AppModule, para que toda a aplicação possa ter acesso. É preciso fazer algumas configurações.

```typescript
import { TypeOrmModule } from "@nestjs/typeorm";

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: "sqlite",
      database: "db.sqlite",
      entities: [],
      synchronize: true,
    }),
  ],
  controllers: [],
  providers: [],
})
export class AppModule {}
```

O método .forRoot({}) serve para configurar algo uma vez, que vai ser usado depois na aplicação. Ele recebe um objeto com algumas propriedades:

1. type: tipo do banco
2. database: nome do banco
3. entities: array de todas as entidades que podem ser armazenadas
4. synchronize: sincroniza as entidades da aplicação com as do banco.

Sqlite é um banco de dados baseado em arquivo. Ao executar a aplicação, o typeorm vai criar o arquivo ('db.sqlite' em questão) na raiz do projeto.
