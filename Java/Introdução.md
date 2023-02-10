Em programas java, os arquivos possuem a extensão '.java'. Cada um pode ter uma classe que tenha o mesmo nome do arquivo. Por exemplo, Main.java pode ter a classe Main. Isso se deve ao fato de que, se ter mais de uma classe, o jdk gera o bytecode para cada arquivo separadamente.

Todo programa java tem um ponto de entrada, que seria um método Main:

```java
public class HelloWorld {
    public static void main(String[] args){
        System.out.println("João Víctor");
    }
}
```

Para declarar uma variável, a sintaxe é: 'tipo' 'nome' = 'valor' (opcional)
Ex:

```java
int idade = 23;
String nome = "João";
```

Se não especificar um valor na atribuição, o java dá um valor padrão. Inteiros recebem 0, Strings recebem null etc.

```java
public class HelloWorld {
    static int idade;
    static String nome;
    public static void main(String[] args){
        System.out.println("Nome: " + nome + ", idade: " + idade + " anos.");
    }
}
```

No entanto, só se forem membros da classe. Dentro de métodos, se não forem inicializados, dá um erro de variável não inicializada.
