Numa aplicação, a estrutura poderia ser assim:

1. Modulo 1 que dependa de módulos 2 e 3
2. Modulos 2 e 3 que dependa de módulo 4 para funcionar

Injeção de dependências entre módulos é um pouco diferente se comparado com a injeção de dependências dentro de um módulo.
Em um módulo seria:

1. Definir a dependência como @Injectable()
2. Adicioná-la na lista de providers
3. Colocá-la dentro do contrutor

Entre módulos seria:

1. Adicionar o código que ser quer compartilhar na lista de exports
2. Importar o módulo no módulo alvo
3. Colocá-lo dentro do construtor

```typescript
import { Module } from "@nestjs/common";
import { PowerService } from "./power.service";

@Module({
  providers: [PowerService],
  exports: [PowerService],
})
export class PowerModule {}
```

É o mesmo que dizer que se quer tornar PowerService disponível para outros módulos.

```typescript
import { Module } from "@nestjs/common";
import { PowerModule } from "src/power/power.module";
import { CpuService } from "./cpu.service";

@Module({
  providers: [CpuService],
  imports: [PowerModule],
})
export class CpuModule {}
```

No módulo alvo, se importa o módulo que o código foi exportado

E no serviço alvo, se adiciona como construtor:

```typescript
import { Injectable } from "@nestjs/common";
import { PowerService } from "src/power/power.service";

@Injectable()
export class CpuService {
  constructor(private powerService: PowerService) {}
}
```

Lembrando que importamos o módulo e dentro se usa o código exportado. Exportar código de um módulo para outro, é o mesmo que dizer que o container de DI "criou" mais uma lista com todas as classes que podem ser usadas em outros módulos.
