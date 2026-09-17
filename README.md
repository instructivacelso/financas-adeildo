# CRM Cobrança & Negociação — Escola Instructiva

Sistema de acompanhamento de negociações de cobrança. Página única (HTML + Tailwind) com
**Firebase Authentication** (login por e-mail/senha) e **Firebase Realtime Database**.

## Deploy no Railway

1. Suba esta pasta em um repositório no GitHub.
2. No Railway: **New Project → Deploy from GitHub repo** → escolha o repositório.
3. O Railway detecta o `package.json` e roda `npm start` automaticamente.
4. Em **Settings → Networking → Generate Domain** para obter a URL pública.

## Configuração no Firebase — feita UMA vez (console.firebase.google.com → projeto `inadimplentes-a249d`)

### 1. Ativar login por e-mail/senha
**Authentication → Sign-in method → Email/Password → Ativar → Salvar.**
(Se a aba Authentication ainda não foi aberta nunca, clique em "Get started" primeiro.)

### 2. Regras do banco (obrigatório — protege os dados de CPF)
**Realtime Database → Rules** → substitua tudo por:

```json
{
  "rules": {
    "crm_users": {
      ".read": "auth != null",
      "$uid": {
        ".write": "auth != null && (!root.child('crm_users').exists() || root.child('crm_users').child(auth.uid).child('role').val() == 'admin')"
      }
    },
    "crm_data": {
      ".read": "auth != null && root.child('crm_users').child(auth.uid).exists()",
      ".write": "auth != null && root.child('crm_users').child(auth.uid).exists()"
    }
  }
}
```

O que isso faz:
- `crm_users` só pode ser criado pelo **primeiro** usuário (bootstrap) ou por um administrador.
- `crm_data` (clientes e negociações) só é lido/escrito por quem está na lista de acessos.

Clique em **Publish**.

Pronto — depois disso tudo é feito dentro do próprio sistema.

## Primeiro acesso

1. Abra o sistema e clique em **"Primeiro acesso? Criar conta de administrador"**.
2. Informe nome, e-mail e senha. Você entra como administrador.
3. Esse link só funciona enquanto não existe nenhum administrador; depois ele recusa.

## Gerenciar a equipe (botão **Equipe**, só para administradores)

- **Criar acesso**: nome (como aparecerá nos cadastros), e-mail, senha inicial e perfil.
- **Tornar admin / atendente**: muda o perfil.
- **Redefinir senha**: envia e-mail de redefinição para a pessoa.
- **Remover acesso**: a pessoa não entra mais; os registros dela ficam no histórico.

> Para o Adeildo continuar como responsável pelos cadastros antigos, crie o acesso dele com
> o nome exatamente igual ao que ele usava antes ("Adeildo").

## Dados existentes

Os cadastros da versão anterior (lista) continuam sendo lidos. Novos cadastros são gravados
por chave individual, permitindo vários atendentes ao mesmo tempo sem sobrescrita.

## Rodar localmente

```bash
npm install
npm start
# abre em http://localhost:3000
```
