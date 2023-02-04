Observando o trecho de código abaixo:

```typescript
@Controller('messages')
export class MessagesController {
  messagesService: MessagesService;

  constructor() {
    this.messagesService = new MessagesService();
  }

  .
  .
  .

}
```

É possível ver que a classe está gerando suas próprias dependências. Toda vez que MessagesController é criada, ela cria a dependência para MessagesService. No entanto, NestJS usa bastante o princípio de inversão de controle, que diz que classes não devem criar suas próprias dependências.

Um outro exemplo de como o código acima poderia ficar seria:

```typescript
@Controller('messages')
export class MessagesController {

    messagesService: MessagesService;
  constructor(service: MessagesService) {
    this.messagesService = service;
  }
  .
  .
  .

}
```

A diferença é que a dependência seria criada de antemão e passada como parâmetro do construtor da classe, e não ela tendo que criar a própria dependência. Mas ainda não é a melhor alternativa, pois desse jeito, ainda fica preso ao tipo específico do MessageService. Uma melhor alternativa seria:

```typescript
interface Repository {
  findAll();
  findOne(id: string);
  create(content: string);
}

export class MessagesService {
  repository: Repository;

  constructor(repo: Repository) {
    this.repository = repo;
  }
}
```

No código acima, se recebe a mesma dependência, mas não precisa especificar que é do tipo MessagesRepository. Poderia se passar qualquer coisa que satisfazesse a implementação da interface. Em testes, por exemplo, pode ter uma implementação 'fake' do repositório, onde não necessariamente faça as operações de gravação, sendo rápido. Em produção, seria uma implementação real.

Sem inversão de controle, para criar um controlador, seria preciso:

1. Criar repositório
2. Criar serviço que recebe como parâmetro o repositório
3. Criar controlador que recebe como o parâmetro o serviço

4. repository = new Repository();
5. service = new Service(repository);
6. controller = new Controller(service);

Aumentando a complexidade, aumentaria o número de linhas de código para garantir que o controlador funcionasse corretamente com o número correto de serviços e repositórios. Para resolver isso, se usa injeção de dependências. É um objeto que basicamente tem duas propriedades: lista de classes e suas dependências e lista das instâncias que ele criou. Ele também é conhecido como Container. Ao iniciar a aplicação, o NestJS cria esse container, analisando as classes que existem (menos os controladores). Por exemplo: ele identifica Um serviço que tem como dependência um repositório, de forma geral existiria uma entrada:

| Classe     | Dependência |
| ---------- | ----------- |
| Service    | Repository  |
| Repository |             |

Ao ver o repositório, não há dependências, por isso fica vazio. Na hora de criar um controlador, ele vai ver o que o controlador precisa, no caso, um serviço. Mas antes, ele vai olhar na lista de classes/dependências e vai ver que ele precisa criar antes um repositório, para poder criar um serviço, para então poder criar o controlador. Ele adiciona as instâncias à lista de instâncias criadas (repository, service).

O fluxo seria:

1. Na inicialização, o container registra todas as classes
2. O container vai ver para cada classe, suas dependências
3. É pedido ao container para que ele crie uma instância de uma classe
4. Container cria as dependências e então, a classe
5. Container guarda as dependências e as reusa quando necessário

Não precisamos registrar controladores, pois eles apenas consomem outras classes. Para registrar as classes no container, é preciso usar o decorator @Injectable():

```typescript
@Injectable()
export class MessagesRepository {}

@Injectable()
export class MessagesService {}
```

Para cada classe de dependência, se decora com @Injectable(), menos os controladores, pois eles não registrados no container.
A última coisa que falta é registrar no módulo, as classes injetáveis (dependências) como Providers:

```typescript
import { Module } from "@nestjs/common";
import { MessagesController } from "./messages.controller";
import { MessagesRepository } from "./messages.repository";
import { MessagesService } from "./messages.service";

@Module({
  controllers: [MessagesController],
  providers: [MessagesService, MessagesRepository],
})
export class MessagesModule {}
```

Pode se entender providers, como classes que podem ser usadas como dependências por outras.

## Observações

1. Quando se fala em criar um controlador, essa tarefa é passada para o container, que então vai ver suas dependências e criar o que for necessário.
2. Quando se fala que o container cria as dependências, ele cria apenas uma instância que é compartilhada entre os usos.

É possível ver isso no código abaixo:

```typescript
constructor(
    private messagesService: MessagesService,
    private messagesService2: MessagesService,
  ) {
    console.log(messagesService === messagesService2);
  }
```

Mesmo se o construtor do controlador pedir por mais de uma instância, o container manda a a mesma que ele criou. É possível também garantir que sempre as dependências sejam instâncias novas e não reaproveitadas.
