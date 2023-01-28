Uma outra forma de se criar aplicações NestJS é instalando o CLI:

~~~bash
npm install -g @nestjs/cli
~~~

E depois utilizando-o:

~~~bash
nest new <nome do projeto>
~~~

O CLI criará um projeto já com todas as dependências necessárias, além de scripts para automatizar processos. Ele permite também a criação automática de módulos e controladores:

~~~bash
nest generate controller <nome>
nest generate module <nome>
~~~

O NestJS tem algumas formas de extrair informações de uma requisição. Por exemplo:

http://localhost:3000/messages/1?validate=true

Na url acima, logo após a barra, vem um parâmetro. O que vem depois da interrogação, constitui a query string. Existem dois decoradores para isso: @Param('id') e @Query(). Para pegar o corpo (body) da requisição, pode-se usar @Body().

~~~typescript
  @Post()
  createMessage(@Body() body: any) {
    return body;
  }

  @Get('/:id')
  getMessage(@Param() id: string) {
    return id;
  }
~~~

Os decoradores são usados na lista de parâmetros e servem para pegar o body e os parâmetros respectivamente.