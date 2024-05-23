# INSTRUÇÕES
## 01) COMECE COM O GUIA DOR EXPRESS 
### Instalação:
Primeiro, você precisa instalar o pacote `express-handlebars` via npm:

```bash
npm install express-handlebars
```

### Configuração com Express.js:
Depois de instalar o pacote, você precisa configurar o Express.js para usar o Handlebars como sua engine de templates. Aqui está um exemplo de como você pode fazer isso:

```javascript
const express = require('express');
const exphbs  = require('express-handlebars');

const app = express();

// Configurando o Handlebars como engine de templates
app.engine('handlebars', exphbs());
app.set('view engine', 'handlebars');
```

### Criando um Template Handlebars:
Agora, vamos criar um template Handlebars simples dentro do diretório `views`. Por exemplo, crie um arquivo chamado `home.handlebars`:

```handlebars
<!-- views/home.handlebars -->

<!DOCTYPE html>
<html>
<head>
    <title>{{ title }}</title>
</head>
<body>
    <h1>{{ message }}</h1>
</body>
</html>
```

### Criando uma Rota para Renderizar o Template:
Agora, crie uma rota no Express.js para renderizar o template Handlebars:

```javascript
app.get('/', (req, res) => {
    res.render('home', { title: 'Página Inicial', message: 'Bem-vindo ao nosso site!' });
});
```

### Iniciando o Servidor:
Por fim, inicie o servidor Express.js para começar a renderizar o template Handlebars:

```javascript
app.listen(3000, () => {
    console.log('Servidor rodando em http://localhost:3000');
});
```

Agora, quando você acessar `http://localhost:3000` no seu navegador, o Express.js renderizará o template Handlebars e enviará a página HTML resultante como resposta. Este é apenas um exemplo simples para começar. Você pode expandir e personalizar isso conforme necessário para o seu projeto.

## 02) CRIAR LAYOUT PADRÃO
Para criar um layout padrão com Handlebars e Express.js, você pode seguir esta abordagem:

### 1. Estrutura do Projeto:
Primeiro, vamos criar uma estrutura de diretórios básica para o nosso projeto:

```
projeto/
│
├── views/
│   ├── layouts/
│   │   └── main.handlebars
│   └── home.handlebars
│
└── server.js
```

### 2. Arquivo de Layout:
Dentro do diretório `views/layouts`, crie um arquivo chamado `main.handlebars`, que servirá como o layout padrão para todas as páginas do seu site. Este arquivo conterá a estrutura HTML básica comum a todas as páginas, como cabeçalho, rodapé, etc.:

```handlebars
<!-- views/layouts/main.handlebars -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ title }}</title>
    <!-- Adicione links para folhas de estilo CSS, scripts JavaScript, etc. aqui, se necessário -->
</head>
<body>
    {{{ body }}}
    <!-- Inclui o conteúdo específico da página renderizada -->
</body>
</html>
```

Observe o uso triplo de chaves `{{{ body }}}`. Isso é onde o conteúdo específico de cada página será injetado.

### 3. Arquivo de Página:
Agora, dentro do diretório `views`, você pode criar os arquivos de página individuais que serão renderizados dentro do layout padrão. Por exemplo, vamos criar um arquivo `home.handlebars`:

```handlebars
<!-- views/home.handlebars -->

<h1>Página Inicial</h1>
<p>Bem-vindo ao nosso site!</p>
```

### 4. Configuração do Express.js:
No arquivo `server.js` (ou qualquer que seja o nome do seu arquivo de servidor), configure o Express.js para usar o Handlebars como sua engine de templates e especifique o layout padrão:

```javascript
const express = require('express');
const exphbs = require('express-handlebars');

const app = express();

// Configuração do Handlebars como engine de templates
app.engine('handlebars', exphbs({ defaultLayout: 'main' }));
app.set('view engine', 'handlebars');

// Rota para a página inicial
app.get('/', (req, res) => {
    res.render('home', { title: 'Página Inicial' });
});

// Iniciar servidor
app.listen(3000, () => {
    console.log('Servidor rodando em http://localhost:3000');
});
```

### 5. Teste:
Agora, quando você iniciar o servidor e acessar `http://localhost:3000` no navegador, o Express.js renderizará a página inicial dentro do layout padrão definido no arquivo `main.handlebars`.

Você pode expandir esse layout padrão adicionando cabeçalhos, rodapés, estilos CSS, scripts JavaScript, etc., conforme necessário para o seu projeto.

## 03) ENVIAR DADOS PARA GUIDÕES EXPRESS
Para enviar dados para os layouts e templates Handlebars renderizados pelo Express.js, você pode passar um objeto contendo os dados desejados como segundo argumento para a função `res.render()`. Aqui está como você pode fazer isso:

Suponha que você tenha um objeto `userData` com informações do usuário que deseja enviar para o template. Você pode passar esse objeto para o template ao renderizá-lo:

```javascript
app.get('/', (req, res) => {
    // Dados do usuário
    const userData = {
        username: 'usuário123',
        email: 'usuario123@email.com',
        idade: 30
    };

    // Renderiza o template 'home' e envia os dados do usuário
    res.render('home', { title: 'Página Inicial', userData: userData });
});
```

Agora, no seu template Handlebars (`home.handlebars`, por exemplo), você pode acessar esses dados usando a sintaxe Handlebars:

```handlebars
<!-- views/home.handlebars -->

<h1>{{ title }}</h1>

<!-- Exibe os dados do usuário -->
<p>Nome de usuário: {{ userData.username }}</p>
<p>Email: {{ userData.email }}</p>
<p>Idade: {{ userData.idade }}</p>
```

Dessa forma, quando você acessar a rota principal (`/`), o Express.js renderizará o template `home.handlebars` e enviará os dados do usuário para serem exibidos no navegador. Este é apenas um exemplo simples de como enviar dados para templates Handlebars com Express.js. Você pode adaptar isso para suas necessidades específicas.

## 04) IF HELPER
Se você precisa adicionar lógica condicional em seus templates Handlebars usando o Express.js, pode usar o helper `if`. Este helper permite executar determinadas ações com base em uma condição.

### Exemplo de Uso:
Suponha que você queira exibir uma mensagem de boas-vindas apenas se o usuário estiver autenticado. Você pode usar o helper `if` para verificar se há um usuário autenticado e exibir a mensagem apropriada de acordo com isso.

No seu arquivo de rota do Express.js:

```javascript
app.get('/', (req, res) => {
    const user = req.user; // Supondo que req.user contenha informações sobre o usuário autenticado
    res.render('home', { title: 'Página Inicial', user: user });
});
```

No seu template Handlebars (`home.handlebars`, por exemplo):

```handlebars
<!-- views/home.handlebars -->

<h1>{{ title }}</h1>

{{#if user}}
    <p>Bem-vindo, {{ user.name }}!</p>
{{else}}
    <p>Por favor, faça login para acessar o conteúdo.</p>
{{/if}}
```

Neste exemplo, o helper `if` é usado para verificar se existe um usuário (`user`) e, se sim, exibe uma mensagem de boas-vindas com o nome do usuário. Se não houver usuário autenticado, exibe uma mensagem solicitando que o usuário faça login.

### Observações:
- O bloco `{{else}}` é opcional e pode ser usado para fornecer um bloco de código a ser executado se a condição no `if` for falsa.
- Você pode usar expressões lógicas mais complexas dentro do `if`, se necessário.
- Certifique-se de que os dados que você está passando para o template contêm as informações necessárias para a lógica condicional.

Este é apenas um exemplo simples de como usar o helper `if` em Handlebars com Express.js. Você pode adaptar isso para suas necessidades específicas de aplicativo.

## 05) UNLESS HELPER
O helper `unless` em Handlebars é o oposto do helper `if`. Ele renderiza o bloco de código dentro dele apenas se a expressão fornecida for avaliada como falsa. Se a expressão for verdadeira, o bloco não será renderizado.

### Exemplo de Uso:
Digamos que você queira exibir uma mensagem de erro apenas se não houver usuários autenticados. Você pode usar o helper `unless` para verificar se não há um usuário autenticado e, em seguida, exibir a mensagem de erro apropriada.

No seu arquivo de rota do Express.js:

```javascript
app.get('/', (req, res) => {
    const user = req.user; // Supondo que req.user contenha informações sobre o usuário autenticado
    res.render('home', { title: 'Página Inicial', user: user });
});
```

No seu template Handlebars (`home.handlebars`, por exemplo):

```handlebars
<!-- views/home.handlebars -->

<h1>{{ title }}</h1>

{{#unless user}}
    <p>Erro: Você precisa estar autenticado para acessar este conteúdo.</p>
{{/unless}}
```

Neste exemplo, o helper `unless` é usado para verificar se não há um usuário autenticado (`user`). Se não houver, ele renderiza o bloco de código dentro dele, exibindo uma mensagem de erro.

### Observações:
- O helper `unless` é útil quando você deseja renderizar um bloco de código apenas se uma condição for falsa.
- Assim como o helper `if`, o bloco `{{else}}` é opcional e pode ser usado para fornecer um bloco de código a ser executado se a condição no `unless` for verdadeira.
- Certifique-se de que os dados que você está passando para o template contêm as informações necessárias para a lógica condicional.

Esse é apenas um exemplo simples de como usar o helper `unless` em Handlebars com Express.js. Você pode adaptar isso para suas necessidades específicas de aplicativo.

## 06) EACH BUILT-IN HELPER
O helper `each` em Handlebars é usado para iterar sobre uma lista de itens e renderizar um bloco de código para cada item na lista.

### Exemplo de Uso:
Suponha que você tenha uma lista de posts e queira renderizar um elemento HTML para cada post.

No seu arquivo de rota do Express.js:

```javascript
app.get('/', (req, res) => {
    const posts = [
        { title: 'Post 1', content: 'Conteúdo do Post 1' },
        { title: 'Post 2', content: 'Conteúdo do Post 2' },
        { title: 'Post 3', content: 'Conteúdo do Post 3' }
    ];
    res.render('home', { title: 'Página Inicial', posts: posts });
});
```

No seu template Handlebars (`home.handlebars`, por exemplo):

```handlebars
<!-- views/home.handlebars -->

<h1>{{ title }}</h1>

<ul>
    {{#each posts}}
        <li>
            <h2>{{ title }}</h2>
            <p>{{ content }}</p>
        </li>
    {{/each}}
</ul>
```

Neste exemplo, o helper `each` é usado para iterar sobre a lista de posts (`posts`). Para cada post na lista, ele renderiza um bloco de código contendo o título e o conteúdo do post.

### Observações:
- Dentro do bloco `each`, você pode acessar as propriedades de cada item usando a sintaxe de chaves duplas (`{{}}`).
- O bloco `each` é especialmente útil para renderizar listas dinâmicas de itens, como posts de um blog, produtos em uma loja online, etc.
- Certifique-se de que os dados que você está passando para o template contêm a lista de itens que você deseja iterar.

Este é apenas um exemplo simples de como usar o helper `each` em Handlebars com Express.js. Você pode adaptar isso para suas necessidades específicas de aplicativo.

## 07) CREATE PARTIALS
Partial é uma seção reutilizável de um template Handlebars que pode ser incluída em outros templates. Isso é útil quando você tem partes de código HTML que são usadas em várias páginas do seu site.

Aqui está como você pode criar e usar parciais em Handlebars com Express.js:

### 1. Criar um Parcial:
Dentro do diretório `views`, crie uma pasta chamada `partials`. Dentro dessa pasta, crie um arquivo para o seu parcial. Por exemplo, vamos criar um parcial chamado `navbar.handlebars`:

```handlebars
<!-- views/partials/navbar.handlebars -->

<nav>
    <ul>
        <li><a href="/">Página Inicial</a></li>
        <li><a href="/about">Sobre</a></li>
        <li><a href="/contact">Contato</a></li>
    </ul>
</nav>
```

Este é um exemplo simples de um parcial de uma barra de navegação.

### 2. Registrar Parciais no Express.js:
No arquivo de configuração do Express.js (por exemplo, `server.js`), você precisa registrar os parciais para que o Handlebars possa encontrá-los. Você pode fazer isso usando o método `registerPartial()` da instância do Handlebars:

```javascript
const express = require('express');
const exphbs = require('express-handlebars');
const path = require('path');

const app = express();

// Configuração do Handlebars como engine de templates
app.engine('handlebars', exphbs({
    defaultLayout: 'main',
    extname: 'handlebars',
    partialsDir: path.join(__dirname, 'views/partials/') // Caminho para a pasta de parciais
}));
app.set('view engine', 'handlebars');
```

### 3. Incluir o Parcial em um Template:
Agora que o parcial está registrado, você pode incluí-lo em qualquer template Handlebars. Por exemplo, você pode incluí-lo no seu layout principal (`main.handlebars`):

```handlebars
<!-- views/layouts/main.handlebars -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ title }}</title>
    <!-- Adicione links para folhas de estilo CSS, scripts JavaScript, etc. aqui, se necessário -->
</head>
<body>
    {{> navbar }} <!-- Inclui o parcial da barra de navegação -->
    {{{ body }}}
    <!-- Inclui o conteúdo específico da página renderizada -->
</body>
</html>
```

A linha `{{> navbar }}` inclui o parcial da barra de navegação no layout principal.

### Observações:
- Certifique-se de que o caminho para a pasta de parciais está configurado corretamente no Express.js para que ele possa encontrar os parciais.
- Os parciais podem ser incluídos em qualquer template Handlebars, não apenas no layout principal. Isso permite reutilização de código em diferentes partes do seu site.

Com isso, você criou e usou com sucesso um parcial em Handlebars com Express.js. 

## 08) CHANGE .HANDLEBARS TO .WHATEVER 
Se você deseja alterar a extensão dos arquivos de template Handlebars de `.handlebars` para qualquer outra extensão, como `.whatever`, você pode fazer isso facilmente na configuração do Express.js.

Aqui está como você pode configurar o Express.js para usar uma extensão personalizada para os arquivos de template Handlebars:

1. No seu arquivo de configuração do Express.js (por exemplo, `server.js`), configure o `extname` para a extensão desejada:

```javascript
const express = require('express');
const exphbs = require('express-handlebars');
const path = require('path');

const app = express();

// Configuração do Handlebars como engine de templates
app.engine('whatever', exphbs({
    defaultLayout: 'main',
    extname: '.whatever', // Extensão personalizada
    layoutsDir: path.join(__dirname, 'views/layouts/'), // Caminho para a pasta de layouts
    partialsDir: path.join(__dirname, 'views/partials/') // Caminho para a pasta de parciais
}));
app.set('view engine', 'whatever'); // Usando a extensão personalizada
```

2. Agora, ao criar seus arquivos de template, você pode usar a extensão personalizada (`.whatever`). Por exemplo, você pode ter `home.whatever`, `about.whatever`, etc., em vez de `home.handlebars`, `about.handlebars`, etc.

Com essa configuração, o Express.js usará a extensão personalizada (`.whatever`) ao renderizar os templates Handlebars.

Lembre-se de ajustar o restante do seu código para usar a nova extensão, incluindo a referência aos arquivos de template em rotas e em outras partes do aplicativo, caso contrário, o Express.js não conseguirá localizar os arquivos de template corretamente.

## 09) LOOKUP BUILT IN HELPER
O helper `lookup` em Handlebars é uma ferramenta poderosa que permite acessar valores em contextos de templates ancestrais. Ele é útil quando você precisa acessar dados de níveis superiores no contexto de um template aninhado.

### Exemplo de Uso:
Suponha que você tenha um contexto de template aninhado e deseje acessar um valor de um nível superior. Por exemplo, você tem um objeto `post` no contexto atual, mas também deseja acessar o `author` do post, que está em um nível superior no contexto.

No seu arquivo de rota do Express.js:

```javascript
app.get('/', (req, res) => {
    const post = {
        title: 'Título do Post',
        content: 'Conteúdo do Post',
        author: {
            name: 'Autor do Post'
        }
    };
    res.render('home', { post: post });
});
```

No seu template Handlebars (`home.handlebars`, por exemplo):

```handlebars
<!-- views/home.handlebars -->

<h1>{{ post.title }}</h1>
<p>{{ post.content }}</p>
<p>Autor: {{ author.name }}</p>
```

Neste exemplo, a linha `{{ author.name }}` tentaria acessar diretamente a propriedade `name` do objeto `author` no contexto atual. No entanto, como `author` está em um nível superior ao contexto atual, isso resultaria em um erro.

Para resolver isso, você pode usar o helper `lookup` para acessar o `author` do contexto superior:

```handlebars
<!-- views/home.handlebars -->

<h1>{{ post.title }}</h1>
<p>{{ post.content }}</p>
<p>Autor: {{ lookup ../ 'author.name' }}</p>
```

Neste exemplo, `../` permite acessar o contexto superior e `'author.name'` é a chave do valor que você deseja acessar. O helper `lookup` então encontrará o valor correspondente a `'author.name'` no contexto superior.

### Observações:
- O helper `lookup` é útil para acessar valores em contextos de templates ancestrais, especialmente em templates aninhados.
- Certifique-se de usar o helper `lookup` com moderação, pois seu uso excessivo pode tornar o código menos legível e mais difícil de entender.

Este é apenas um exemplo simples de como usar o helper `lookup` em Handlebars com Express.js. Você pode adaptar isso para suas necessidades específicas de aplicativo.

## 10) CREATE CUSTOM HELPER
Para criar um helper personalizado em Handlebars com Express.js, você pode registrar uma função que define o comportamento do helper desejado. Aqui está como você pode fazer isso:

### Passo 1: Registrar o Helper Personalizado
No arquivo de configuração do Express.js (por exemplo, `server.js`), você pode registrar um helper personalizado usando o método `registerHelper()` da instância do Handlebars. Este método aceita o nome do helper e uma função que define o comportamento do helper:

```javascript
const express = require('express');
const exphbs = require('express-handlebars');
const path = require('path');

const app = express();

// Configuração do Handlebars como engine de templates
app.engine('handlebars', exphbs({
    defaultLayout: 'main',
    extname: '.handlebars',
    layoutsDir: path.join(__dirname, 'views/layouts/'),
    partialsDir: path.join(__dirname, 'views/partials/')
}));
app.set('view engine', 'handlebars');

// Registrar helper personalizado
const Handlebars = require('handlebars');
Handlebars.registerHelper('capitalize', function(str) {
    // Converte a primeira letra da string para maiúscula
    return str.charAt(0).toUpperCase() + str.slice(1);
});
```

### Passo 2: Usar o Helper Personalizado
Agora que o helper personalizado está registrado, você pode usá-lo em seus templates Handlebars. Por exemplo, você pode usá-lo em qualquer template Handlebars:

```handlebars
<!-- views/home.handlebars -->

<h1>{{ capitalize title }}</h1>
```

Neste exemplo, `{{ capitalize title }}` usa o helper personalizado `capitalize` para converter a primeira letra do título em maiúscula.

### Observações:
- No exemplo acima, o helper personalizado `capitalize` recebe um parâmetro `str` (a string que você deseja capitalizar) e retorna a string com a primeira letra em maiúscula.
- O nome do helper personalizado (`capitalize`) pode ser qualquer coisa que você desejar. Ele será usado no template para chamar o helper.

Este é apenas um exemplo simples de como criar e usar um helper personalizado em Handlebars com Express.js. Você pode adaptar isso para criar helpers que atendam às suas necessidades específicas de aplicativo.

## 11) {{ }} VS {{{ }}}
Em Handlebars, há uma diferença importante entre `{{ }}` e `{{{ }}}` em termos de escape de HTML.

- `{{ }}`: Este é o bloco de expressão padrão em Handlebars. Ele escapa HTML por padrão, o que significa que qualquer HTML dentro do bloco será tratado como texto e renderizado como tal no navegador. Por exemplo:

```handlebars
{{ title }}
```

Se `title` conter HTML, ele será renderizado como texto e não como HTML no navegador.

- `{{{ }}}`: Este é o bloco de expressão "triplo" em Handlebars. Ele não escapa HTML, o que significa que qualquer HTML dentro do bloco será renderizado como HTML válido no navegador. Por exemplo:

```handlebars
{{{ title }}}
```

Se `title` conter HTML, ele será renderizado como HTML no navegador.

### Quando usar cada um?
- Use `{{ }}` quando você deseja que o texto seja tratado como texto e não como HTML. Isso é útil para evitar injeções de HTML não seguras em seu aplicativo.
  
- Use `{{{ }}}` quando você deseja renderizar HTML real. Isso é útil quando você confia na origem do conteúdo e deseja renderizar o HTML conforme fornecido.

É importante usar o escape HTML apropriado para evitar vulnerabilidades de segurança, como ataques de injeção de scripts (XSS). Certifique-se de entender a origem do conteúdo que você está renderizando e escolher o método de escape HTML apropriado com base nisso.

## 12) WITH BUILT IN HELPER
O helper `with` em Handlebars é usado para mudar o contexto de renderização dentro de um bloco de código. Isso é útil quando você precisa acessar propriedades aninhadas de um objeto sem repetir o nome do objeto em cada linha.

### Exemplo de Uso:
Suponha que você tenha um objeto `post` com várias propriedades, incluindo um objeto `author` aninhado, e queira acessar várias propriedades do `author`. Você pode usar o helper `with` para mudar o contexto de renderização para o objeto `author` dentro de um bloco de código:

No seu arquivo de rota do Express.js:

```javascript
app.get('/', (req, res) => {
    const post = {
        title: 'Título do Post',
        content: 'Conteúdo do Post',
        author: {
            name: 'Autor do Post',
            bio: 'Bio do Autor'
        }
    };
    res.render('home', { post: post });
});
```

No seu template Handlebars (`home.handlebars`, por exemplo):

```handlebars
<!-- views/home.handlebars -->

<h1>{{ title }}</h1>
<p>{{ content }}</p>

{{#with author}}
    <p>Nome do Autor: {{ name }}</p>
    <p>Bio do Autor: {{ bio }}</p>
{{/with}}
```

Neste exemplo, o bloco `with` muda o contexto de renderização para o objeto `author`. Dentro desse bloco, você pode acessar as propriedades `name` e `bio` diretamente, sem repetir `author` em cada linha.

### Observações:
- O bloco `with` é útil para simplificar o código quando você precisa acessar várias propriedades de um objeto aninhado.
- Dentro do bloco `with`, você pode acessar propriedades diretamente sem usar o nome do objeto pai.
- Certifique-se de usar o bloco `with` com moderação e entender como ele afeta o contexto de renderização.

Este é apenas um exemplo simples de como usar o helper `with` em Handlebars com Express.js. Você pode adaptar isso para suas necessidades específicas de aplicativo.

## 13) MULTIPLE CSS FILES WITH
Para incluir vários arquivos CSS em um template Handlebars com Express.js, você pode usar a tag HTML `<link>` dentro do seu layout principal e especificar o caminho para cada arquivo CSS desejado. Aqui está como você pode fazer isso:

### 1. No seu layout principal (por exemplo, `main.handlebars`):
```handlebars
<!-- views/layouts/main.handlebars -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ title }}</title>
    <!-- Link para o primeiro arquivo CSS -->
    <link rel="stylesheet" href="/css/style1.css">
    <!-- Link para o segundo arquivo CSS -->
    <link rel="stylesheet" href="/css/style2.css">
    <!-- Outros metadados e links para CSS, se necessário -->
</head>
<body>
    {{{ body }}}
    <!-- Inclui o conteúdo específico da página renderizada -->
</body>
</html>
```

### 2. Organize seus arquivos CSS:
Certifique-se de ter seus arquivos CSS (`style1.css`, `style2.css`, etc.) em um diretório acessível pelo seu servidor. No exemplo acima, os arquivos CSS estão no diretório `/css`, mas você pode organizá-los como preferir.

### 3. Configuração de Roteamento no Express.js:
No seu arquivo de configuração do Express.js (por exemplo, `server.js`), você precisa configurar o Express.js para servir arquivos estáticos, incluindo seus arquivos CSS. Aqui está um exemplo:

```javascript
const express = require('express');
const exphbs = require('express-handlebars');
const path = require('path');

const app = express();

// Configuração do Handlebars como engine de templates
app.engine('handlebars', exphbs({
    defaultLayout: 'main',
    extname: '.handlebars',
    layoutsDir: path.join(__dirname, 'views/layouts/'),
    partialsDir: path.join(__dirname, 'views/partials/')
}));
app.set('view engine', 'handlebars');

// Configuração para servir arquivos estáticos (como arquivos CSS)
app.use(express.static(path.join(__dirname, 'public')));

// Rotas
app.get('/', (req, res) => {
    res.render('home', { title: 'Página Inicial' });
});

// Iniciar servidor
app.listen(3000, () => {
    console.log('Servidor rodando em http://localhost:3000');
});
```

### 4. Organize seus arquivos estáticos:
Certifique-se de ter seus arquivos CSS (e outros arquivos estáticos, se houver) em um diretório acessível pelo seu servidor. No exemplo acima, os arquivos CSS estão no diretório `/public/css`, mas você pode organizá-los como preferir.

Com essas etapas, você pode incluir múltiplos arquivos CSS em seus templates Handlebars com Express.js. Certifique-se de ajustar os caminhos dos arquivos CSS e dos diretórios estáticos conforme necessário para a estrutura do seu projeto.

## 14) PROJETO FINAL
- [ACESSE O "./CODIGO" PARA USAR O PROJETO FINAL](./CODIGO/)
- [ESSE PROJETO JA FOI PUBLICADO NO GITHUB COM ALGUMAS MELHORIAS](https://github.com/VILHALVA/BURGUER-APP)