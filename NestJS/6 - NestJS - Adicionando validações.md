É possível adicionar validações às requisições. Um exemplo é a 'NotFoundException':

```typescript

import { NotFoundException } from '@nestjs/common';

@Get('/:id')
  async getMessage(@Param('id') id: string) {
    const message = await this.messagesService.findOne(id);

    if (!message) {
      throw new NotFoundException('Message Not Found');
    }

    return message;
  }
```

Se passa como argumento a mensagem desejada.
