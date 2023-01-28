## NestJS

NestJS é um framework para criação de aplicações web escaláveis utilizando typescript, que foi construída em cima do Express, um outro famoso framework para criação de aplicações web/apis. Sua filosofia é que atualmente, não existe uma forma padronizada de se arquitetar uma aplicação web, pois com o avanço de Javascript, os arquivos poderiam estar em qualquer lugar, sem nenhuma padronização.

---------------------------------------
## Bibliotecas padrão do NestJS

~~~json
"dependencies": {
    "@nestjs/common": "^7.6.17",
    "@nestjs/core": "^7.6.17",
    "@nestjs/platform-express": "^7.6.17",
    "reflect-metadata": "^0.1.13",
    "typescript": "^4.3.2"
  }
~~~

Common: classes e funções padrão do Nest
Core: implementação padrão do Nest
Platform-express: permite usar o express para lidar com requisições HTTP
reflect-metadata: permite a criação de anotações (decoradores)
typescript: linguagem usada no desenvolvimento

Por padrão, o Nestjs depende de uma implementação de um gerenciador de requisições HTTP, que podem ser duas: Express ou Fastify. Uma definição de tsconfig.json para definir as configurações de Typescript de um projeto padrão de Nestjs seria:

~~~json
{
    "compilerOptions": {
        "module": "commonjs",
        "target": "ES2017",
        "experimentalDecorators": true,
        "emitDecoratorMetadata": true
    }
}
~~~~

O NestJS permite fazer várias ações do lado do servidor:

1. Validar dados do request por meio de pipes
2. Garantir que o usuário esteja autenticado por meio de um Guard
3. Rotear uma requisição para uma função específica por meio de um controlador
4. Executar lógica de negócio por meio de um serviço
5. Acessar um banco de dados
6. Usar filtros para lidar com erros que ocorrem durante a requisição
7. Usar de interceptadores para adicionar lógica extra na requisição ou na resposta

---------------------------------------
## Estrutura básica de uma aplicação NestJS

Uma aplicação NestJS consiste basicamente de módulos, que possuem controladores para lidar com as requisições.

~~~typescript
import { Controller, Get, Module } from "@nestjs/common";
import { NestFactory } from "@nestjs/core";

@Controller("app")
class AppController {
  @Get()
  getRoute() {
    return "Oi!";
  }
}

@Module({
  controllers: [AppController],
})
class AppModule {}

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3333);
}
bootstrap();
~~~~

Toda aplicação tem um ponto de entrada, que vai inicializar tudo através de um método geralmente chamado de bootstrap. Ele chama o NestFactory e invoca o método create, passando um módulo principal de entrada. Esse módulo geralmente carrega as demais dependências da aplicação.

Um controlador é uma classe Typescript que é decorada com @Controller(), que serve para dizer que aquela classe é um controlador que vai possuir métodos diversos que podem ser decorados com os verbos HTTP (@Get()).

Cada coisa é um arquivo separado.

1. main.ts -> serve para inicializar a aplicação
2. app.module.ts -> indica o módulo 
3. app.controller.ts -> indica o controlador daquele módulo 'app'.

Nos decoradores Controller e Get, ao passar uma string como parâmetro:

~~~typescript
@Controller("/app")
class AppController {
  @Get('/metodo')
  getRoute() {
    return "Oi!";
  }
}
~~~

A requisição precisará conter essas strings: localhost:3000/app/metodo.