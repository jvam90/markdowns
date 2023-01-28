Pipes servem para adicionar validações extras durante o percurso da requisição. Elas podem ser em uma única rota, ou na aplicação inteira:

```typescript
import { NestFactory } from "@nestjs/core";
import { ValidationPipe } from "@nestjs/common";
import { MessagesModule } from "./messages/messages.module";

async function bootstrap() {
  const app = await NestFactory.create(MessagesModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
```

Aqui, se configura para que a aplicação (app) use pipes com um ValidationPipe.

São quatro passos:

1. Configurar o NestJS para usar validação global
2. Criar uma classe que represente a requisição (body), ou um DTO
3. Adicionar validações à classe
4. Aplicar essa classe no gerenciador de requisições

Para adicionar as validações, é preciso de uma biblioteca chamada class-validator:

```bash
npm install class-validator
```

E o DTO ficaria assim:

```typescript
import { IsString } from "class-validator";

export class CreateMessageDto {
  @IsString()
  content: string;
}
```

Basta adicionar um decorador à propriedade que se quer validar. No caso, se content é uma string (IsString()). E no controlador:

```typescript
@Post()
  createMessage(@Body() body: CreateMessageDto) {
    return body;
  }
```

Ao tentar fazer uma requisição com dados inválidos, deve aparecer uma mensagem padrão:

```json
{
  "statusCode": 400,
  "message": ["content must be a string"],
  "error": "Bad Request"
}
```

Um DTO é um objeto de transferência de dados. Apenas leva dados de um ponto a outro, geralmente de um servidor para uma aplicação (backend => frontend). Em Javascript/Typescript, existem dois tipos de objetos: apenas dados e com construtores e métodos de acesso. É possível transformar objetos JSON puros em classes com a biblioteca class-transformer:

```bash
npm install class-transformer
```

O fluxo de validação seria:

1. Receber o body e converter com o class-transformer para um DTO especificado
2. Usar o class-validator para validar
3. Se algo estiver errado, responder de acordo

Mas como o NestJS sabe que ao receber um body, ele deve validar/converter para o tipo especificado no handler, que no exemplo abaixo seria CreateMessageDto?

```typescript
@Post()
  createMessage(@Body() body: CreateMessageDto) {
    return body;
  }
```

No arquivo tsconfig.json, existem duas configurações:

```json
"emitDecoratorMetadata": true,
"experimentalDecorators": true,
```

O primeiro é o que permite que as informações dos tipos sejam passados do Typescript para o Javascript, já que uma vez transpilado, as informações de tipos são removidas. Na pasta dist, existem os arquivos Javascript gerados. Em particular, no arquivo do controlador representado no código acima:

```javascript
__decorate(
  [
    (0, common_1.Post)(),
    __param(0, (0, common_1.Body)()),
    __metadata("design:type", Function),
    __metadata("design:paramtypes", [create_message_dto_1.CreateMessageDto]),
    __metadata("design:returntype", void 0),
  ],
  MessagesController.prototype,
  "createMessage",
  null
);
```

É uma função que vai "aplicar decoradores" no Javascript (eles não existem naturalmente). É possível ver que se aplica o decorador Body ao primeiro (0) parâmetro do método, além do Post em cima da função que lida com a requisição e que ele espera um parâmetro do tipo CreateMessageDto.
