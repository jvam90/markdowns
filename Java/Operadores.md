É possível realizar operações matemáticas com valores e variáveis do código. As 5 operações básicas são:

1. adição +
2. subtração -
3. multiplicação \*
4. divisão /
5. módulo (ou resto da divisão) %

```java
public static void main(String[] args){
    int num1 = 10, num2 = 5;
    System.out.println("Adição 10 + 5: " + (10 + 5));
    System.out.println("Subtração 10 -5: " + (10 - 5));
    System.out.println("Multiplicação 10 * 5: " + (10 * 5));
    System.out.println("Divisão: " + (10 / 5));
    System.out.println("Módulo: " + (10 % 5));
}
```

Lembrando que deve respeitar a precisão dos tipos. Por exemplo, se dividir um inteiro por outro inteiro, dá inteiro. Se for um inteiro por um double, o java converte para double. Double por um inteiro dá double. O java sempre tenta manter a precisão mais alta se os tipos forem compatíveis. E a operação deve ser entre tipos compatíveis. Um inteiro não pode ser somado com uma string, por exemplo.

```java
int num1 = 10;
String teste = "teste";
int num3 = num1 + teste; //incompatible types: java.lang.String cannot be converted to int
```

Outro operador bastante usado é a atribuição (=), que é atribuir valor a algo, geralmente variáveis.
Existe também a atribuição de forma curta:

```java
int num = 10;
num = num + 10; //forma longa
num += 10;//forma curta
num -= 10;
num *= 10;
num /= 10;
```
