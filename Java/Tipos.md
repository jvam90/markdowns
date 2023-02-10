Java contém dois tipos de dados: primitivos e não-primitivos.

Primitivos:

1. byte
2. short
3. int
4. long (precisa por um l no final para o compilador entender que é long)
5. float (precisa por um f no final para o compilador entender que é float)
6. double
7. boolean
8. char

Não primitivos:

1. String
2. Arrays
3. Classes

Tipos não primitivos guardam referências para uma posição de memória que contém o valor, e não o valor em si, diferente dos tipos primitivos.

O java também é capaz de converter de um tipo para outro de duas formas: implícita e explícita.

Implícita: quando se tenta converter de um tipo com menos precisão, para um tipo com mais precisão. Por exemplo, de int para double:

```java
public static void main(String[] args){
        int num = 5;
        double num2 = num;
        System.out.println(num2); //imprime 5.0
    }
```

É como se tirasse de uma 'caixa' menor, para por em uma maior. No entanto, se for o oposto:

```java
public static void main(String[] args){
        double num = 5;
        int num2 = num;
        System.out.println(num2); //incompatible types: possible lossy conversion from double to int
    }
```

Seria o oposto: tirar de uma precisão maior para uma menor. Nisso, pode haver perda de dados, por isso o java não permite fazer a conversão implícita. Isso é possível com uma conversão explícita:

```java
 public static void main(String[] args){
        double num = 5;
        int num2 = (int)num;
        System.out.println(num2);
}
```

Ele vai perder a precisão do double, mas consegue converter de um tipo para outro.
