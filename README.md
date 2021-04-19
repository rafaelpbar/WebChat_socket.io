## Desenvolvimento

Deverá ser desenvolvida uma aplicação `Node.js` de chat, usando `socket.io` para emitir eventos e atualizar estado no servidor e cliente.

Através do cliente será possível, enviar e receber mensagens, trocar seu nome, ver usuários online. 

O MVC será usado para renderizar as mensagens do histórico e usuários online, com ambos vindo direto do servidor. 

## DURANTE O DESENVOLVIMENTO

- ⚠ **RECOMENDAMOS QUE VOCÊ FIQUE ATENTO ÀS ISSUES DO CODE CLIMATE, PARA RESOLVÊ-LAS ANTES DE FINALIZAR O DESENVOLVIMENTO.** ⚠

## Linter (Análise Estática) 

Para garantir a qualidade do código, usaremos o [ESLint](https://eslint.org/) para fazer a sua análise estática.

Este projeto já vem com as dependências relacionadas ao _linter_ configuradas nos arquivos `package.json` nos seguintes caminhos:

- `sd-05-project-webchat/package.json`

Para poder rodar os `ESLint` em um projeto basta executar o comando `npm install` dentro do projeto e depois `npm run lint`. Se a análise do `ESLint` encontrar problemas no seu código, tais problemas serão mostrados no seu terminal. Se não houver problema no seu código, nada será impresso no seu terminal.

Você pode também instalar o plugin do `ESLint` no `VSCode`, bastar ir em extensions e baixar o [plugin `ESLint`](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint).

## Variáveis

Haverá um arquivo no caminho: `sd-05-project-webchat/tests/helpers/db.js`. Neste arquivo, na linha 10, Haverá a seguinte comando:

`.connect(process.env.DB_URL, {`

e na linha 14:

`.then((conn) => conn.db(process.env.DB_NAME))`

   **Você irá precisar configurar as variáveis globais do MongoDB.** Você pode usar esse [Conteúdo de variáveis de ambiente com NodeJS](https://blog.rocketseat.com.br/variaveis-ambiente-nodejs/) como referência.

**(Neste arquivo e obrigatório deixar o nome do database como `webchat`)**

## Conexão com o banco:
Ela deve ter o seguinte formato:
```javascript
const { MongoClient } = require('mongodb');
require('dotenv').config();

let schema = null;

async function connection() {
  if (schema) return Promise.resolve(schema);
  return MongoClient
    .connect(process.env.DB_URL || 'mongodb://localhost:27017/webchat', {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    })
    .then((conn) => conn.db(process.env.DB_NAME))
    .then((dbSchema) => {
      schema = dbSchema;
      return schema;
    })
    .catch((err) => {
      console.error(err);
      process.exit(1);
    });
}
module.exports = connection;
```

**Com elas que iremos conseguir conectar ao banco do avaliador automático**

---
# Requisitos do projeto
## Lista de Requisitos

### 1 - Crie um back-end para conexão simultaneamente de clientes e troca de mensagens em chat público

- Sua aplicação deve ser inicializada no arquivo `server.js`;

- Seu back-end deve permitir que várias usuários se conectem simultâneamente;

- Seu back-end deve permitir que usuários mandem mensagens para todos os outros usuários de forma simultânea;

- Toda mensagem que um cliente recebe deve conter as informações acerca de quem a enviou, data-hora do envio e o conteúdo da mensagem em si. A data-hora das mensagens deve ser determinada pelo momento em que são salvas no banco de dados (ver requisito 3);

 - O evento da mensagem deve ter o nome `message` e deve enviar como parâmetro o objeto `{ chatMessage, nickname }`. O `chatMessage` deve ser o mensagem enviada e o `nickname` o nickname do usuário que a enviou;

 - A data na mensagem deve seguir o padrão 'dd-mm-yyyy' e o horário deve seguir o padrão 'hh:mm:ss' sendo os segundos opcionais;

 - O formato da mensagem deve seguir esse padrão:

 `DD-MM-yyyy HH:mm:ss ${message.nickname} ${message.chatMessage}`

- Exemplo prático:

`09-10-2020 2:35:09 PM - Joel: Olá meu caros amigos!`

- O servidor deve enviar a mensagem ao front-end **já formatada**, ela deve ser uma `string`, como nos dois exemplos acima; 


#### As seguintes verificações serão feitas:

- Será validado que vários clientes conseguem se conectar ao mesmo tempo;

- Será validado que cada cliente conectado ao chat recebe todas as mensagens que já foram enviadas;

- Será validado que toda mensagem que um cliente recebe contém as informações acerca de quem a enviou, data-hora do envio e o conteúdo da mensagem em si;

### 2 - Crie um frontend para que as pessoas interajam com o chat.

- O front-end e o back-end tem que usar a mesma porta a `localhost:3000`;

- As mensagens devem ser renderizadas na tela;
 - Cada mensagem deve ter o `data-testid="message"`.

- O frontend deve exibir todas as mensagens já enviadas no chat, ordenadas verticalmente da mais antiga para a mais nova;

- O frontend deve ter uma caixa de texto através da qual quem usa consiga enviar mensagens para o chat:
  - A caixa de texto deve conter o `data-testid="message-box"`.

  - O botão de enviar mensagem deve conter o `data-testid="send-button"`.

- O front-end deve permitir a quem usa escolher um apelido (_nickname_) para si. Para que o cliente consiga escolher um apelido deve ter um campo de texto e um botão no front-end. O campo de texto será onde o cliente digitará o _nickname_ que deseja. Após escolher o _nickname_, o cliente deverá clicar no botão para que o dado seja salvo:
  - o campo onde o nickname será inserido deve conter o `data-testid="nickname-box"`.

  - o botão que será clicado para salvar o nickname deve conter `data-testid="nickname-save"`.

  - ao entrar, o usuário deve receber um nickname aleatório.

#### As seguintes verificações serão feitas:

- Será validado que o frontend tem uma caixa pra enviar mensagens;

- Será validado que o frontend possui um campo onde o usuário pode inserir o nickname e um botão para salvar;

- Será validado que é possível enviar mensagem após alterar o nickname;

### 3 - Elabore o histórico do chat para que as mensagens persistirão.

- Você deve configurar um banco de dados MongoDB, onde cada linha contém uma mensagem enviada, e a coleção deve se chamar `messages`;

- O seu banco de dados deve salvar o nickname de quem enviou a mensagem, a mensagem em si e uma _timestamp_ com precisão de segundos de quando ela foi salva no banco;

- Envie o histórico de mensagens corrente via html quando respondida a requisição, use uma template engine para isso; 

#### As seguintes verificações serão feitas:

- Será validado que todo o histórico de mensagens irá aparecer quando o cliente se conectar;

- Será validado que ao enviar uma mensagem e recarregar a página , a mensagem persistirá;

- Será validado que o histórico é enviado junto ao html, ou seja, usando MVC.

### 4 - Informe a todos os clientes quem está online no momento.

- No front-end deve haver uma lista, na tela de cada cliente, que mostra quais clientes estão online em um dado momento. Um cliente é identificado pelo seu _nickname_.
  - O elemento com o nome do cliente deve conter o `data-testid="online-user"`

  - O cliente atual sempre dever seu o nome como primeiro da primeiro da lista

  - Quando um cliente se conecta, ele deve entrar no final da lista de clientes online

  - Usuários online devem ser enviados no html e atualizados via socket

#### As seguintes verificações serão feitas:

- Será validado que quando um usuário se conecta, seu nome aparece no frontend de todos

- Será validado que quando um usuário se desconecta, seu nome desaparece do frontend dos outros usuários.

- Será validado que os usuários online são enviados junto ao html, ou seja, usando MVC.

## Requisitos Bônus

### 5 - Permita que usuários troquem mensagens particulares.

- No frontend deve haver uma lista com todos os clientes e, ao lado de cada identificador, um botão. Um clique nesse botão deve direcionar as pessoas para um chat privado. Além disso, deve existir um botão para entrar no chat público. 
  - O usuário não deve conseguir enviar mensagens privadas para si mesmo.

  - O botão para o chat privado deve ter o `data-testid="private"` 

  - O botão para o chat público deve ter o `data-testid="public"`

- No front-end deve ser possível navegar entre os chats privados ou o chat geral na mesma janela, clicando no botão com `data-testid="private"` para ir ao privado e `data-testid="public"` para voltar para o público.

- Mensagens particulares só devem ser visíveis para as partes pertinentes. Clientes terceiros não devem poder acessar seu conteúdo.

 - o formato da mensagem deve seguir esse padrão:

 `DD-MM-yyyy HH:mm:ss (private) ${message.nickname} ${message.chatMessage}`

- exemplo prático:

`09-10-2020 2:35:09 (private) - Joel: Olá meu caros amigos!`


#### As seguintes verificações serão feitas:

- Será validado que existe o botão para entrar em um chat privado e para voltar ao chat principal;

- Será validado que é possível enviar uma mensagem privada para um usuário.

- Será validado que o cliente da sessão é o primeiro usuário da lista de onlines

- Será validado que é possível transitar entre o chat particular e o chat global.


