Serviços e repositórios são classes, mas com funcionalidades distintas.

Um serviço é onde se coloca lógica de negócio e onde se consome um repositório para recuperação ou armazenamento de dados.

Um repositório é onde se tem a lógica de comunicação com o banco de dados, através de um esquema, orm, ou SQL puro.

Num serviço, a implementação poderia ficar assim:

```typescript
import { MessagesRepository } from "./messages.repository";

export class MessagesService {
  messagesRepo: MessagesRepository;
  constructor() {
    this.messagesRepo = new MessagesRepository();
  }

  async findOne(id: string) {}

  async findAll() {}

  async create(content: string) {}
}
```

No entanto, assim, o próprio serviço cria suas próprias dependências. No caso, pra ele funcionar, depende de uma instância de MessagesRepository.
