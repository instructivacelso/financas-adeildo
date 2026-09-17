# CRM Cobrança & Negociação — Escola Instructiva

Sistema de acompanhamento de negociações de cobrança. Página única (HTML + Tailwind) com
**Firebase Authentication** (login por e-mail/senha) e **Firebase Realtime Database**.

## Deploy no Railway

1. Suba esta pasta em um repositório no GitHub.
2. No Railway: **New Project → Deploy from GitHub repo** → escolha o repositório.
3. O Railway detecta o `package.json` e roda `npm start` automaticamente.
4. Em **Settings → Networking → Generate Domain** para obter a URL pública.

## Configuração obrigatória no Firebase (console.firebase.google.com → projeto `inadimplentes-a249d`)

### 1. Ativar login por e-mail/senha
**Authentication → Sign-in method → Email/Password → Ativar.**

### 2. Criar as contas dos atendentes
**Authentication → Users → Add user** (e-mail + senha). O nome exibido no sistema é a parte
do e-mail antes do `@` (ex.: `adeildo@...` → "Adeildo"). Para usar nomes com mais de uma
palavra, o registro antigo do atendente continua funcionando pelo nome que ele já tinha.

### 3. Autorizar o domínio do Railway
**Authentication → Settings → Authorized domains → Add domain** → cole o domínio gerado pelo
Railway (ex.: `crm-cobranca.up.railway.app`). Sem isso o login é bloqueado.

### 4. Proteger o banco (IMPORTANTE — dados de CPF)
**Realtime Database → Rules** → substitua por:

```json
{
  "rules": {
    "crm_data": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```

Com isso só quem estiver logado lê ou escreve.

## Dados existentes

Os cadastros feitos na versão anterior (armazenados como lista) continuam sendo lidos
normalmente. Novos cadastros são gravados por chave individual, o que permite vários
atendentes trabalharem ao mesmo tempo sem sobrescrever uns aos outros.

## Rodar localmente

```bash
npm install
npm start
# abre em http://localhost:3000
```
