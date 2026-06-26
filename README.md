# Gull Finance — Agenda

Sistema de notificações automáticas via WhatsApp, integrado ao Firebase Realtime Database.
**Projeto 100% independente** — Firebase próprio, repositório próprio, deploy próprio.

---

## Estrutura

```
gull-finance-agenda/
├── public/
│   └── index.html            ← Aplicação completa
├── .github/
│   └── workflows/
│       └── deploy.yml        ← Auto-deploy no push para main
├── netlify.toml              ← Config do Netlify
├── firebase-rules.json       ← Regras de segurança do Firebase
└── .gitignore
```

---

## Passo 1 — Firebase (novo projeto)

1. Acesse https://console.firebase.google.com
2. Clique em **"Criar projeto"** → nome: `gull-finance-agenda`
3. Ative o **Realtime Database**:
   - Vá em Build → Realtime Database → Create Database
   - Escolha a região (us-central1 ou southamerica-east1)
   - Inicie em **modo de teste** (vamos aplicar as regras depois)
4. Ative a **Authentication**:
   - Vá em Build → Authentication → Get started
   - Ative o provedor **E-mail/Senha**
   - Crie os usuários que terão acesso
5. Vá em **Configurações do projeto (⚙)** → "Seus aplicativos" → Adicionar app Web (`</>`)
   - Nome: `gull-finance-agenda-web`
   - Copie o objeto `firebaseConfig` gerado

6. Cole as credenciais em `public/index.html`, substituindo o bloco:
```js
const firebaseConfig = {
  apiKey:            "COLE_AQUI_SUA_API_KEY",
  authDomain:        "gull-finance-agenda.firebaseapp.com",
  databaseURL:       "https://gull-finance-agenda-default-rtdb.firebaseio.com",
  projectId:         "gull-finance-agenda",
  ...
};
```

7. Aplique as regras de segurança:
   - Realtime Database → Regras → cole o conteúdo de `firebase-rules.json` → Publicar

---

## Passo 2 — GitHub

```bash
git init
git add .
git commit -m "init: gull-finance-agenda"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/gull-finance-agenda.git
git push -u origin main
```

---

## Passo 3 — Netlify

1. Acesse https://app.netlify.com → **Add new site → Import an existing project → GitHub**
2. Selecione o repositório `gull-finance-agenda`
3. Configure:
   - **Build command:** *(vazio)*
   - **Publish directory:** `public`
4. Clique em **Deploy site**

### Secrets para o GitHub Actions

No Netlify:
- **Site ID:** Site configuration → General → Site ID
- **Auth Token:** User settings → Applications → New access token

No GitHub (Settings → Secrets → Actions):
- `NETLIFY_SITE_ID` → valor do Site ID
- `NETLIFY_AUTH_TOKEN` → valor do token

---

## Passo 4 — Configurar o app

Após o deploy, acesse o site e vá na aba **Configurações**:

| Campo | Descrição |
|---|---|
| URL da API | URL da sua Evolution API (ex: `https://api.empresa.com`) |
| API Key | Chave de autenticação da Evolution API |
| Nome da instância | Nome da instância WhatsApp conectada |

Clique em **Buscar grupos** para listar os grupos disponíveis.

---

## Deploy automático

```bash
# Após qualquer mudança:
git add .
git commit -m "update: descrição"
git push
# → GitHub Actions faz o deploy no Netlify automaticamente
```

---

## Estrutura de dados (Firebase)

```
gull-finance-agenda/
├── eventos/
│   └── {id}/
│       ├── titulo, data, hora, ts
│       ├── grupoJid, grupoNome
│       ├── obs, status, done
│       ├── criadoPor, criadoEm
│       └── notificadoCriacao, notificadoCriacaoEm
└── historico/
    └── {id}/
        ├── tipo (criacao | conclusao)
        ├── eventoId, titulo, grupoNome
        ├── msg, status (ok | erro)
        └── criadoPor, criadoEm
```
