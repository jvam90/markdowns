Relembrando:

1. Requisição chega -> pipe vai validar os dados
2. Passando, ele passa para o controlador para uma determinada função
3. O controlador manda para um serviço, que vai rodar lógica de negócio
4. O serviço vai chamar um repositório

Para testar os controladores, basta fazer requisições HTTP por meio de um serviço, como postman ou thunder.

## NOTA: requisição com @Param precisa ter o parâmetro especificado:

```typescript
@Get('/:id')
  getMessage(@Param('id') id: string) {
    return this.messagesService.findOne(id);
  }
```
