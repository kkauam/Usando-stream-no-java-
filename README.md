# 🌊 Java Streams — Projeto de Estudo

> *"Streams não são coleções. São pipelines de processamento de dados."*

---

## 📌 Sobre o Projeto

Este projeto foi desenvolvido com o objetivo de **praticar e entender o uso da Stream API do Java**, introduzida no **Java 8**. Aqui você encontrará exemplos práticos de como utilizar streams para filtrar, mapear, ordenar e reduzir dados de forma elegante e funcional.

O projeto lê dados de funcionários a partir de um arquivo `.csv`, carrega-os em uma lista de objetos `Employee`, e realiza operações diversas usando streams.

---

## 🧠 O que é a Stream API?

Imagine que você tem uma lista de pessoas e quer:

1. Filtrar apenas quem ganha mais de R$ 5.000,00
2. Pegar só o e-mail dessas pessoas
3. Ordenar alfabeticamente

Sem streams, você precisaria de vários `for` loops, variáveis auxiliares e muito código repetitivo. **Com Streams, você faz tudo isso em 4 linhas.**

### Conceito Central

Uma **Stream** é uma sequência de elementos que suporta operações agregadas. Ela **não armazena dados** — ela os processa. Pense em uma linha de produção em uma fábrica:

```
[Fonte de dados] → [Filtro] → [Transformação] → [Ordenação] → [Resultado]
```

---

## 🔧 Como funciona na prática?

### 1. Criando uma Stream

```java
List<Employee> lista = new ArrayList<>();

// A partir de uma lista:
lista.stream()

// A partir de valores diretos:
Stream.of("Ana", "Bruno", "Carlos")

// A partir de um array:
Arrays.stream(meuArray)
```

---

### 2. Operações Intermediárias *(processam e retornam outra Stream)*

Essas operações são **"lazy"** — elas só executam quando uma operação terminal é chamada.

#### 🔍 `.filter()` — Filtra elementos
```java
// Retorna apenas funcionários com salário maior que o informado
list.stream()
    .filter(x -> x.getSalary() > salary)
```

#### 🔄 `.map()` — Transforma elementos
```java
// Pega o e-mail de cada funcionário
.map(x -> x.getEmail())

// Forma reduzida com Method Reference:
.map(Employee::getEmail)
```

#### 🔤 `.sorted()` — Ordena elementos
```java
// Ordena em ordem natural (alfabética para Strings, numérica para números)
.sorted()

// Ordena com critério personalizado:
.sorted(Comparator.comparing(Employee::getName))
```

#### 🚫 `.distinct()` — Remove duplicatas
```java
.distinct()
```

#### ✂️ `.limit()` — Limita a quantidade
```java
.limit(10) // Retorna apenas os 10 primeiros
```

---

### 3. Operações Terminais *(encerram o pipeline e retornam um resultado)*

#### 📦 `.collect()` — Coleta em uma coleção
```java
// Coleta em uma lista
.collect(Collectors.toList())

// Coleta em um Set (sem duplicatas):
.collect(Collectors.toSet())
```

#### ➕ `.reduce()` — Reduz a um único valor
```java
// Soma os salários de todos os funcionários
.reduce(0.0, (x, y) -> x + y)

// O primeiro argumento (0.0) é o valor inicial (identidade)
// (x, y) -> x + y é a operação que acumula o resultado
```

#### 🖨️ `.forEach()` — Itera sobre cada elemento
```java
emails.forEach(System.out::println)
```

#### 🔢 `.count()` — Conta os elementos
```java
long total = lista.stream().filter(...).count();
```

#### ✅ `.anyMatch()` / `.allMatch()` / `.noneMatch()`
```java
boolean algumGanhaAlto = lista.stream()
    .anyMatch(x -> x.getSalary() > 10000);
```

---

## 💻 Exemplos do Projeto

### Buscar e-mails de quem ganha mais que um salário informado

```java
List<String> emails = list.stream()
    .filter(x -> x.getSalary() > salary)   // Filtra por salário
    .map(x -> x.getEmail())                // Extrai o e-mail
    .sorted()                              // Ordena alfabeticamente
    .collect(Collectors.toList());         // Coleta em lista
```

---

### Somar salários de quem tem nome começando com 'M'

```java
double sum = list.stream()
    .filter(x -> x.getName().charAt(0) == 'M')  // Filtra nomes com 'M'
    .map(x -> x.getSalary())                     // Extrai os salários
    .reduce(0.0, (x, y) -> x + y);              // Soma tudo
```

---

## 🗂️ Estrutura do Projeto

```
exStream/
├── src/
│   ├── application/
│   │   └── Program.java        # Classe principal com os exemplos
│   └── entities/
│       └── Employee.java       # Entidade com nome, e-mail e salário
└── employees.csv               # Arquivo de dados de entrada
```

---

## 📄 Formato do CSV

```
João Silva,joao@email.com,8500.00
Maria Souza,maria@email.com,12000.00
Pedro Lima,pedro@email.com,4300.00
```

---

## ⚡ Por que usar Streams?

| Sem Stream | Com Stream |
|---|---|
| Vários `for` loops | Pipeline limpo e legível |
| Código verboso | Código conciso e expressivo |
| Difícil de paralelizar | `.parallelStream()` paraleliza automaticamente |
| Mistura de "o quê" e "como" | Foca no **"o quê"**, não no "como" |

---

## 📚 Pré-requisitos

- Java 8 ou superior
- IDE de sua preferência (IntelliJ IDEA, Eclipse, VS Code)

---

## ▶️ Como executar

1. Clone o repositório:
```bash
git clone https://github.com/seu-usuario/exStream.git
```

2. Certifique-se de que o arquivo `employees.csv` está no local correto (caminho configurado no `Program.java`)

3. Execute a classe `Program.java`

4. Informe o salário quando solicitado pelo console

---

## 🎯 Aprendizados-chave

- Stream **não modifica** a coleção original
- Operações intermediárias são **lazy** (preguiçosas)
- Operações terminais são **eager** (disparam o processamento)
- Uma stream só pode ser **consumida uma vez**
- Use `.parallelStream()` com cuidado em ambientes com estado compartilhado

---

## 👨‍💻 Autor

Desenvolvido como projeto de estudo da **Stream API do Java**.

---

> 💡 **Dica do professor:** Sempre que você sentir vontade de escrever um `for` com `if` dentro, pare e pense: *"Isso não seria melhor com um `.filter()` e um `.map()`?"* — A resposta quase sempre é sim. 😄
