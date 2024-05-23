# MANUAL
## INSTALAÇÃO:
Para começar a usar o Handlebars com Express, você precisa instalar o pacote `express-handlebars`. Você pode fazer isso via npm:

```bash
npm install express-handlebars
```

## CONFIGURAÇÃO COM EXPRESS.JS:
Depois de instalar o pacote, você precisa configurar o Express.js para usar o Handlebars como sua engine de templates. Aqui está um exemplo de como você pode fazer isso:

```javascript
const express = require('express');
const exphbs  = require('express-handlebars');

const app = express();

// Configurando o Handlebars como engine de templates
app.engine('handlebars', exphbs());
app.set('view engine', 'handlebars');

// Rota de exemplo
app.get('/', (req, res) => {
    res.render('home', { title: 'Página Inicial', message: 'Bem-vindo ao nosso site!' });
});

app.listen(3000, () => {
    console.log('Servidor rodando em http://localhost:3000');
});
```

## EXEMPLO DE TEMPLATE HANDLEBARS:
Agora, você pode criar arquivos `.handlebars` dentro do diretório `views` para definir seus templates. Aqui está um exemplo simples:

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

Neste exemplo, `{{ title }}` e `{{ message }}` são marcadores de posição que serão preenchidos pelos valores fornecidos na função `res.render()`.

## PASSANDO DADOS PARA O TEMPLATE:
Ao usar `res.render()`, você pode passar um objeto contendo dados que serão usados no template Handlebars. Por exemplo:

```javascript
app.get('/', (req, res) => {
    res.render('home', { title: 'Página Inicial', message: 'Bem-vindo ao nosso site!' });
});
```

No exemplo acima, `title` e `message` são passados para o template Handlebars e podem ser usados lá.

