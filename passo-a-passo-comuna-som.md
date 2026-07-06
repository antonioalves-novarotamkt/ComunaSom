# Comuna Som — Passo a passo pra colocar o Mapa de Palco no ar

Esse guia cobre tudo, do zero: criar a conta gratuita no Firebase, ligar o banco de dados, ativar o login, e colocar o site no ar com GitHub Pages.

---

## Parte 1 — Criar o projeto no Firebase

1. Acesse **https://console.firebase.google.com** e entre com uma conta Google (pode ser a mesma do dia a dia, ou criar uma só pra empresa).
2. Clique em **"Criar um projeto"** (ou "Adicionar projeto").
3. Dê o nome **`comuna-som`** (ou o nome que preferir).
4. Na etapa do Google Analytics, pode **desativar** — não é necessário para o mapa de palco.
5. Clique em **"Criar projeto"** e aguarde.

---

## Parte 2 — Ativar o banco de dados (Firestore)

1. No menu lateral esquerdo, vá em **Compilação → Firestore Database**.
2. Clique em **"Criar banco de dados"**.
3. Escolha **"Iniciar em modo de teste"**.
4. Escolha a localização **`southamerica-east1`** (São Paulo), que é a mais próxima do Brasil.
5. Clique em **"Ativar"**.

### Ajustar as regras de segurança
1. Ainda dentro do Firestore Database, clique na aba **"Regras"** (no topo).
2. Apague o conteúdo e cole exatamente isto:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /stagePlots/{plotId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

3. Clique em **"Publicar"**.

Isso deixa qualquer pessoa **ver** os mapas, mas só quem estiver **logado** pode alterar.

---

## Parte 3 — Ativar o login (Authentication)

1. No menu lateral, vá em **Compilação → Authentication**.
2. Clique em **"Vamos começar"**.
3. Na aba **"Sign-in method"** (Método de login), clique em **"E-mail/senha"**.
4. Ative a primeira opção (**"E-mail/senha"**) e clique em **"Salvar"**.

### Criar as contas de quem vai editar
1. Vá na aba **"Users"** (Usuários), logo ao lado de "Sign-in method".
2. Clique em **"Add user"** (Adicionar usuário).
3. Preencha um e-mail e uma senha — essa vai ser a conta que você (ou seu técnico) vai usar pra entrar no modo edição do site.
4. Repita pra cada pessoa da equipe que precisar editar os mapas.

> Não existe cadastro público — só você cria essas contas por aqui, uma por uma.

---

## Parte 4 — Pegar as chaves de configuração

1. Clique no ícone de **engrenagem** (⚙️) no topo do menu lateral → **"Configurações do projeto"**.
2. Role a página até a seção **"Seus aplicativos"**.
3. Clique no ícone **`</>`** (Web).
4. Dê um apelido ao app, por exemplo `comuna-som-web`, e clique em **"Registrar app"**.
5. Vai aparecer um bloco de código parecido com isto:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "comuna-som-xxxx.firebaseapp.com",
  projectId: "comuna-som-xxxx",
  storageBucket: "comuna-som-xxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

6. **Copie esse bloco inteiro.**
7. Abra o arquivo `index.html` (o do mapa de palco) em qualquer editor de texto simples (Bloco de Notas, Notepad++, VS Code).
8. Procure por este trecho, perto do início do `<script>`:

```javascript
const firebaseConfig = {
  apiKey: "COLE_AQUI_SUA_API_KEY",
  authDomain: "COLE_AQUI.firebaseapp.com",
  projectId: "COLE_AQUI",
  storageBucket: "COLE_AQUI.appspot.com",
  messagingSenderId: "COLE_AQUI",
  appId: "COLE_AQUI"
};
```

9. **Apague esse trecho e cole o bloco que você copiou do Firebase no lugar.**
10. Salve o arquivo.

---

## Parte 5 — Colocar no ar (GitHub Pages)

1. Crie uma conta em **https://github.com** (se ainda não tiver).
2. Clique em **"New repository"**. Nome sugerido: `comuna-som-mapa-palco`. Marque como **Public**. Crie.
3. Dentro do repositório, clique em **"Add file" → "Upload files"**.
4. Arraste estes 5 arquivos juntos:
   - `index.html`
   - `manifest.json`
   - `service-worker.js`
   - `icon-192.png`
   - `icon-512.png`
5. Clique em **"Commit changes"**.
6. Vá em **Settings → Pages** (menu lateral do repositório).
7. Em "Branch", escolha **`main`** e a pasta **`/ (root)`** → **Save**.
8. Aguarde 1–2 minutos. O link vai aparecer ali mesmo, algo como:
   ```
   https://seu-usuario.github.io/comuna-som-mapa-palco/
   ```

Esse é o link que você compartilha com a equipe e com os clientes.

---

## Resumo de como usar no dia a dia

- **Qualquer pessoa** que abrir o link escolhe um mapa em "Mapas publicados..." e visualiza — sem conseguir alterar nada.
- **Você (ou quem tiver conta)** clica em **"🔒 Entrar pra editar"**, digita e-mail e senha, e libera a edição completa (arrastar itens, modo cabo, salvar).
- Ao clicar em **"Salvar"**, o mapa fica publicado na nuvem na hora — quem estiver vendo aquele mapa (mesmo sem login) atualiza sozinho.

## Se precisar adicionar mais alguém pra editar depois
Volte em **Authentication → Users → Add user** no Firebase e crie a conta da pessoa. Não precisa mexer em mais nada no código.
