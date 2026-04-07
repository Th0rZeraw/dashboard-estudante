# EstudoFlow — Guia de Deploy no GitHub Pages

## O que foi mudado?

O backend original dependia de um servidor (`/api/auth`, `/api/data`) que **não funciona** no GitHub Pages.
A solução foi integrar o **Firebase** (Google) — gratuito, sem servidor, dados sincronizados em tempo real entre celular e PC.

---

## Passo a passo completo

### 1. Criar projeto no Firebase

1. Acesse [console.firebase.google.com](https://console.firebase.google.com)
2. Clique em **"Adicionar projeto"**
3. Dê um nome (ex: `estudoflow`) e clique em **Continuar**
4. Desative o Google Analytics (opcional) e clique em **Criar projeto**

---

### 2. Ativar Authentication (Login por e-mail)

1. No menu lateral, clique em **Authentication**
2. Clique em **"Começar"**
3. Na aba **"Sign-in method"**, clique em **E-mail/senha**
4. Ative a primeira opção e clique em **Salvar**

---

### 3. Criar o Realtime Database

1. No menu lateral, clique em **Realtime Database**
2. Clique em **"Criar banco de dados"**
3. Escolha a localização (pode deixar padrão) e clique em **Próximo**
4. Selecione **"Iniciar no modo de teste"** e clique em **Ativar**

> ⚠️ O modo de teste expira em 30 dias. Antes de expirar, vá em **Regras** e substitua por:
> ```json
> {
>   "rules": {
>     "users": {
>       "$uid": {
>         ".read":  "$uid === auth.uid",
>         ".write": "$uid === auth.uid"
>       }
>     }
>   }
> }
> ```

---

### 4. Pegar a configuração do Firebase

1. No menu lateral, clique no ícone de engrenagem ⚙️ → **Configurações do projeto**
2. Role até **"Seus apps"** e clique no ícone **`</>`** (Web)
3. Dê um apelido e clique em **Registrar app**
4. Copie o bloco `firebaseConfig` que aparece:

```js
const firebaseConfig = {
  apiKey: "AIza...",
  authDomain: "seu-projeto.firebaseapp.com",
  databaseURL: "https://seu-projeto-default-rtdb.firebaseio.com",
  projectId: "seu-projeto",
  storageBucket: "seu-projeto.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

---

### 5. Colar a configuração no index.html

Abra o `index.html` e localize este trecho (perto do topo):

```js
const firebaseConfig = {
  apiKey:            "COLE_AQUI",
  authDomain:        "COLE_AQUI",
  databaseURL:       "COLE_AQUI",
  projectId:         "COLE_AQUI",
  storageBucket:     "COLE_AQUI",
  messagingSenderId: "COLE_AQUI",
  appId:             "COLE_AQUI"
};
```

Substitua cada `"COLE_AQUI"` pelo valor correspondente do seu projeto.

---

### 6. Publicar no GitHub Pages

1. Crie um repositório no GitHub (pode ser público ou privado)
2. Suba os arquivos:
   - `index.html`
   - `favicon.svg`
3. Vá em **Settings → Pages**
4. Em **"Source"**, selecione a branch `main` e pasta `/ (root)`
5. Clique em **Save**

Em alguns minutos o site estará disponível em:
`https://SEU_USUARIO.github.io/NOME_DO_REPOSITORIO`

---

### 7. Autorizar o domínio no Firebase

> Sem isso, o login vai falhar!

1. No Firebase, vá em **Authentication → Settings → Authorized domains**
2. Clique em **"Add domain"**
3. Digite: `SEU_USUARIO.github.io`
4. Clique em **Add**

---

## Pronto! 🎉

- Crie sua conta pelo site → clique em "Registrar nova conta"
- Use **e-mail e senha** para entrar
- Os dados ficam salvos automaticamente na nuvem
- Funciona igual no celular e no PC

---

## Plano gratuito do Firebase (Spark)

| Recurso | Limite gratuito |
|---------|----------------|
| Authentication | 10.000 logins/mês |
| Realtime Database | 1 GB armazenado, 10 GB/mês transferência |
| Usuários simultâneos | 100 conexões |

Para uso pessoal, o plano gratuito é mais do que suficiente.
