# Quiz de Fatos Históricos

Projeto desenvolvido em **JavaScript** com execução via **terminal**, utilizando o pacote `readline-sync` para entrada de dados do usuário.

O objetivo do projeto é praticar conceitos fundamentais de JavaScript, como:

- Arrays
- Objetos
- Funções
- Arrow functions
- Laços de repetição
- Condicionais
- Entrada de dados pelo terminal
- Organização de código
- Manipulação de strings
- Controle de pontuação

---

## Sobre o projeto

O projeto é um jogo de perguntas e respostas sobre fatos históricos.

O programa seleciona algumas perguntas de forma aleatória, exibe uma pergunta por vez no terminal e aguarda a resposta do jogador.

Após cada resposta, o programa informa se o jogador acertou ou errou.

No final, o sistema exibe:

- Nome do jogador
- Pontuação final
- Mensagem personalizada de desempenho

---

## Tecnologias utilizadas

- JavaScript
- Node.js
- readline-sync

---

## Como executar o projeto

### 1. Instalar o Node.js

Verifique se o Node.js está instalado:

```bash
node -v
```

Se aparecer uma versão, por exemplo:

```bash
v20.11.0
```

significa que o Node.js está instalado corretamente.

---

### 2. Criar o projeto

Crie uma pasta para o projeto:

```bash
mkdir quiz-fatos-historicos
```

Entre na pasta:

```bash
cd quiz-fatos-historicos
```

Inicialize o projeto Node.js:

```bash
npm init -y
```

---

### 3. Instalar o readline-sync

```bash
npm install readline-sync
```

---

### 4. Configurar o package.json

Como o projeto usa `import`, é necessário adicionar `"type": "module"` no arquivo `package.json`.

Exemplo:

```json
{
  "name": "quiz-fatos-historicos",
  "version": "1.0.0",
  "type": "module",
  "main": "index.js",
  "dependencies": {
    "readline-sync": "^1.4.10"
  }
}
```

---

### 5. Executar o projeto

No terminal, execute:

```bash
node index.js
```

---

## Estrutura do projeto

```bash
quiz-fatos-historicos/
│
├── index.js
├── package.json
├── package-lock.json
└── README.md
```

---

## Fluxo do sistema

```txt
INÍCIO
  |
  v
Exibe o título do quiz
  |
  v
Solicita o nome do jogador
  |
  v
Seleciona perguntas aleatórias
  |
  v
Exibe uma pergunta
  |
  v
Usuário digita a resposta
  |
  v
Verifica se a resposta está correta
  |
  v
Atualiza pontuação
  |
  v
Ainda existem perguntas?
  |
  |--- SIM ---> Exibe próxima pergunta
  |
  |--- NÃO ---> Exibe resultado final
  |
  v
FIM
```

---

## Componentes do projeto

### 1. Array de questões

Responsável por armazenar todas as perguntas do quiz.

Cada pergunta possui:

- `id`
- `pergunta`
- `resposta`

Exemplo:

```js
const questoes = [
  {
    id: 1,
    pergunta: 'Quando aconteceu o atentado as Torres Gêmeas?',
    resposta: '2001'
  },
  {
    id: 2,
    pergunta: 'Em que ano foi detectado o primeiro paciente com coronavírus?',
    resposta: '2019'
  }
];
```

---

### 2. Função `exibirTitulo`

Responsável por exibir o título do jogo no terminal.

```js
const exibirTitulo = () => {
  console.log('=================================');
  console.log('     QUIZ DE FATOS HISTÓRICOS');
  console.log('=================================\n');
};
```

---

### 3. Função `receberNome`

Responsável por solicitar e armazenar o nome do jogador.

```js
function receberNome() {
  console.log('Seja Bem Vindo(a)!\n');

  let nome = EntradaDados.question('Digite seu Nome: ').trim();

  while (nome === '') {
    console.log('Nome inválido. Digite novamente.');
    nome = EntradaDados.question('Digite seu Nome: ').trim();
  }

  return nome;
}
```

Essa função usa `trim()` para remover espaços antes e depois do nome digitado.

Exemplo:

```txt
" Vanessa " vira "Vanessa"
```

---

### 4. Função `embaralharArray`

Responsável por embaralhar as perguntas.

```js
function embaralharArray(colecaoPerguntas) {
  return [...colecaoPerguntas].sort(() => Math.random() - 0.5);
}
```

O uso de:

```js
[...colecaoPerguntas]
```

cria uma cópia do array original.

Isso evita alterar diretamente o array principal `questoes`.

---

### 5. Função `selecionarQuestoesAleatorias`

Responsável por selecionar uma quantidade específica de perguntas.

```js
const selecionarQuestoesAleatorias = (colecaoPerguntas, quantidade) => {
  const perguntasEmbaralhadas = embaralharArray(colecaoPerguntas);
  return perguntasEmbaralhadas.slice(0, quantidade);
};
```

Exemplo:

```js
selecionarQuestoesAleatorias(questoes, 3);
```

Nesse caso, o programa seleciona apenas 3 perguntas aleatórias.

---

### 6. Função `fazerPerguntas`

Responsável por:

- Exibir cada pergunta
- Receber a resposta do usuário
- Verificar se está correta
- Atualizar a pontuação

```js
function fazerPerguntas(perguntasSelecionadas) {
  let pontuacao = 0;

  perguntasSelecionadas.forEach((questao, index) => {
    console.log(`\nPergunta ${index + 1}: ${questao.pergunta}`);

    const respostaUsuario = EntradaDados.question('Sua Resposta: ').trim();

    if (respostaUsuario === questao.resposta) {
      console.log('RESPOSTA CERTA!');
      pontuacao++;
    } else {
      console.log('RESPOSTA ERRADA! - A resposta correta era: ' + questao.resposta);
    }
  });

  return pontuacao;
}
```

---

### 7. Função `verificarMensagemFinal`

Responsável por definir a mensagem final de acordo com o desempenho do jogador.

```js
function verificarMensagemFinal(pontuacao, totalPerguntas) {
  const percentual = (pontuacao / totalPerguntas) * 100;

  if (percentual < 40) {
    return 'OH NÃO! - Tente mais uma vez!';
  } else if (percentual < 60) {
    return 'BOM TRABALHO! - Pratique um pouco mais!';
  } else if (percentual < 90) {
    return 'MUITO BOM! - Você acertou a maioria!';
  } else {
    return 'EXCELENTE!!! - Você é um verdadeiro expert!';
  }
}
```

---

### 8. Função `exibirResultados`

Responsável por mostrar o resultado final do quiz.

```js
function exibirResultados(nomeJogador, pontuacao, totalPerguntas) {
  const mensagemFinal = verificarMensagemFinal(pontuacao, totalPerguntas);

  console.log('\n=================================');
  console.log('        RESULTADO FINAL');
  console.log('=================================');
  console.log(mensagemFinal);
  console.log(`Jogador(a): ${nomeJogador}`);
  console.log(`Pontuação final: ${pontuacao} acertos de ${totalPerguntas}.`);
}
```

---

### 9. Função `iniciarQuiz`

Função principal do projeto.

Ela organiza a ordem de execução do programa.

```js
function iniciarQuiz() {
  const quantidadePerguntas = 3;

  exibirTitulo();

  const nomeJogador = receberNome();

  const perguntasSelecionadas = selecionarQuestoesAleatorias(
    questoes,
    quantidadePerguntas
  );

  const pontuacao = fazerPerguntas(perguntasSelecionadas);

  exibirResultados(nomeJogador, pontuacao, quantidadePerguntas);
}

iniciarQuiz();
```

---

## Exemplo do programa rodando

```txt
=================================
     QUIZ DE FATOS HISTÓRICOS
=================================

Seja Bem Vindo(a)!

Digite seu Nome: Vanessa

Pergunta 1: Em que ano começou a Primeira Guerra Mundial?
Sua Resposta: 1914
RESPOSTA CERTA!

Pergunta 2: Qual o ano que o Titanic afundou?
Sua Resposta: 1910
RESPOSTA ERRADA! - A resposta correta era: 1912

Pergunta 3: Em que ano o homem pisou na Lua pela primeira vez?
Sua Resposta: 1969
RESPOSTA CERTA!

=================================
        RESULTADO FINAL
=================================
MUITO BOM! - Você acertou a maioria!
Jogador(a): Vanessa
Pontuação final: 2 acertos de 3.
```

---

## Conceitos de JavaScript praticados

### Arrays

Usado para armazenar várias questões.

```js
const questoes = [];
```

---

### Objetos

Cada questão é representada por um objeto.

```js
{
  id: 1,
  pergunta: 'Texto da pergunta',
  resposta: '2001'
}
```

---

### Funções

Usadas para organizar o projeto em partes menores.

```js
function receberNome() {
  return nome;
}
```

---

### Arrow functions

Também usadas para criar funções de forma mais curta.

```js
const exibirTitulo = () => {
  console.log('QUIZ');
};
```

---

### Condicionais

Usadas para verificar se a resposta está correta.

```js
if (respostaUsuario === questao.resposta) {
  pontuacao++;
} else {
  console.log('Resposta errada');
}
```

---

### Laço `forEach`

Usado para percorrer as perguntas selecionadas.

```js
perguntasSelecionadas.forEach((questao, index) => {
  console.log(questao.pergunta);
});
```

---

### Método `sort`

Usado para embaralhar o array.

```js
.sort(() => Math.random() - 0.5)
```

---

### Método `slice`

Usado para selecionar apenas uma parte do array.

```js
.slice(0, quantidade)
```

---

### Método `trim`

Usado para remover espaços extras da resposta do usuário.

```js
EntradaDados.question('Sua Resposta: ').trim();
```

---

## Melhorias futuras

Algumas melhorias que podem ser feitas futuramente:

- Adicionar perguntas de múltipla escolha
- Permitir escolher a quantidade de perguntas
- Criar níveis de dificuldade
- Separar perguntas por categoria
- Criar ranking de jogadores
- Salvar resultados em arquivo
- Criar uma versão com HTML, CSS e JavaScript
- Criar uma interface gráfica na web
- Adicionar botão de reiniciar quiz
- Adicionar temporizador para responder cada pergunta

---

## Possível evolução para múltipla escolha

Hoje a pergunta possui apenas a resposta correta:

```js
{
  pergunta: 'Quando aconteceu o atentado as Torres Gêmeas?',
  resposta: '2001'
}
```

No futuro, pode virar:

```js
{
  pergunta: 'Quando aconteceu o atentado as Torres Gêmeas?',
  opcoes: {
    a: '1999',
    b: '2000',
    c: '2001',
    d: '2002'
  },
  resposta: 'c'
}
```

Assim o usuário responderia apenas:

```txt
a, b, c ou d
```

---

## Objetivo de aprendizado

Este projeto foi criado com o objetivo de praticar JavaScript de forma simples e progressiva.

A primeira versão roda no terminal, mas a estrutura foi pensada para facilitar uma futura migração para uma página web.

A lógica do quiz está separada em funções, o que facilita manutenção, leitura e evolução do projeto.

---

## Status do projeto

Projeto em desenvolvimento...

Versão atual: 1.0

```txt
Quiz via terminal com perguntas aleatórias, pontuação e resultado final.
```

```txt
Quiz de múltipla escolha com opções A, B, C e D.
```