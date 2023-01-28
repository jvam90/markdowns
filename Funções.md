
O sitema de tipos do Typescript permite além de anotar/inferir tipos de variáveis, fazer o mesmo com funções, seus parâmetros e tipos de retorno.

~~~typescript
function dizerAlgo(algo){
    console.log(algo)
}
~~~

Em Typescript, se passar o cursor sobre a função, é possível ver alguns erros/alertas:

~~~typescript
Parameter 'algo' implicitly has an 'any' type.
~~~

Como não há tipo explícito, ele assume any. E como não tem retorno na função, considera como void.

Podemos dizer os tipos assim como em declarações de variáveis:

~~~typescript
function dizerAlgo(algo: string){
    console.log(algo)
}
~~~

Por padrão, Typescript requer que todos os parâmetros especificados sejam passados:

~~~typescript
function dizerAlgo(algo: string, algo2: string){
    console.log(algo, algo2)
}
dizerAlgo("oi") //-   Expected 2 arguments, but got 1.
~~~

Se quisermos especificar parâmetros opcionais, devemos usar ?:

~~~typescript
function funcao (algo: string, algo2?: string){
	//algo aqui
}
~~~

Se algo2 não for passado, ele assume como undefined. Seu tipo será uma união:

~~~typescript
(parameter) algo2: string | undefined
~~~

Agora por padrão, um padrão opcional não pode seguir um padrão obrigatório:

~~~typescript
function funcao (algo?: string, algo2: string){
	// A required parameter cannot follow an optional parameter.
}
~~~

Para definir parâmetros padrão, depois do seu tipo, podemos definir um valor padrão, caso ele não seja passado.

~~~typescript
function dizerAlgo(algo: string, algo2: string = "valor padrão"){
    console.log(algo, algo2)
}
~~~

Caso o segundo parâmetro não seja passado, o Typescript assume como valor padrão a ser usado a string "valor padrão".

Uma outra característica de parâmetros em Javascript/Typescript, é que eles podem ser "rest", ou seja, podem representar um número infinito de valores passados:

~~~typescript
function imprimir(algo: string, ...algo2: string[]){
    console.log(algo);
    for(let a of algo2){
        console.log(a);
    }
}

imprimir("primeiro", "segundo", "terceiro", "quarto")
~~~

No caso, algo2 é representada por um array de strings com o operador spread do Javascript, que é a mesma coisa que dizer (com arrays):

~~~typescript
function imprimir(algo: string, algo2: string, algo3: string, algo4: string){
    console.log(algo);
    for(let a of algo2){
        console.log(a);
    }
}
~~~

Typescript pode inferir tipos de retorno:

~~~typescript
function somar(a: number, b: number){
    return a + b;
}
~~~

Pelos parâmetros, ele infere que o retorno vai ser number, já que dentro da função estão sendo somados. Do mesmo jeito, o tipo do retorno pode ser explícito:

~~~typescript
function somar(a: number, b: number) : number{
    return a + b;
}
~~~

O que é desnecessário se usarmos a inferência de tipos. No entanto, há algumas situações onde pode ser bom deixar de forma explícita:

