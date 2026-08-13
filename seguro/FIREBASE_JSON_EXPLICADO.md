# 📋 Explicação do firebase.json

O arquivo `firebase.json` é a **configuração central** do Firebase. Define como fazer deploy e quais serviços usar.

## 📝 Estrutura Completa

```json
{
  // ========================================
  // 1️⃣ PROJETOS
  // ========================================
  "projects": {
    // Projeto padrão (preencha com ID do seu projeto)
    "default": "seu-projeto-cafe-12345"
  },

  // ========================================
  // 2️⃣ FIRESTORE (Banco de Dados)
  // ========================================
  "firestore": {
    // Localização do arquivo com as regras de segurança
    "rules": "firestore.rules",
    
    // Índices para otimizar consultas
    "indexes": "firestore.indexes.json"
  },

  // ========================================
  // 3️⃣ HOSTING (Site Web)
  // ========================================
  "hosting": {
    // Pasta com arquivos estáticos
    // Observação: colocar "." para raiz do projeto
    "public": "public",

    // Arquivos/pastas a IGNORAR no deploy
    "ignore": [
      "firebase.json",      // Config (não fazer upload)
      "**/.*",             // Arquivos ocultos (.gitignore, .env, etc)
      "**/node_modules/**" // Dependências (não upload)
    ],

    // ========================================
    // REWRITE RULES
    // ========================================
    "rewrites": [
      {
        // Se o URL não corresponder a nenhum arquivo estático
        "source": "**",
        
        // Redirecione para index.html
        // (Necessário para Single Page Applications)
        "destination": "/index.html"
      }
    ]
  }
}
```

---

## 🔑 Campos Importantes

### 1. `projects.default`
```json
"projects": {
  "default": "cafe-app-123"
}
```
- ID do seu projeto no Firebase
- Encontre em: Firebase Console → Configurações → ID do Projeto
- Necessário para todos os deploys

### 2. `firestore.rules`
```json
"firestore": {
  "rules": "firestore.rules"
}
```
- Caminho para arquivo com regras de segurança
- Define permissões de acesso ao banco de dados
- Deploy com: `firebase deploy --only firestore:rules`

### 3. `firestore.indexes`
```json
"firestore": {
  "indexes": "firestore.indexes.json"
}
```
- Índices para otimizar consultas
- Gerado automaticamente pelo Firebase
- Importante para performance

### 4. `hosting.public`
```json
"hosting": {
  "public": "public"  // ou "." para usar raiz
}
```
- Pasta com arquivos para fazer upload
- Normalmente é `public/` ou raiz do projeto
- Apenas esses arquivos serão servidos

### 5. `hosting.ignore`
```json
"ignore": [
  "firebase.json",      // Não suba config
  "**/.*",             // Não suba arquivos ocultos
  "**/node_modules/**" // Não suba node_modules
]
```
- Padrões de arquivos a **ignorar** no upload
- Economiza espaço e tempo de deploy

### 6. `hosting.rewrites`
```json
"rewrites": [
  {
    "source": "**",              // Todos os URLs
    "destination": "/index.html" // Redirecione para SPA
  }
]
```
- Redireciona URLs não encontradas para index.html
- Essencial para Single Page Applications (SPA)
- Permite que React/Vue/Angular gerenciem rotas

---

## 🗂️ Estrutura Esperada

```
firebase.json          ← Configuração do Firebase
firestore.rules        ← Regras de segurança
firestore.indexes.json ← Índices (gerado automaticamente)

.env                   ← Credenciais (NÃO fazer upload)
.env.example          ← Template

public/
├── index.html        ← Página principal
├── style.css         ← Estilos
├── main.js          ← JavaScript
└── images/          ← Imagens e assets

node_modules/         ← Dependências (ignorado)
```

---

## 🚀 Exemplos de Configurações

### Exemplo 1: SPA React
```json
{
  "projects": {
    "default": "meu-app-react"
  },
  "hosting": {
    "public": "build",
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

### Exemplo 2: Site Estático
```json
{
  "projects": {
    "default": "meu-site"
  },
  "hosting": {
    "public": ".",
    "ignore": ["firebase.json", ".git/**"]
  }
}
```

### Exemplo 3: Com Rewrite para API
```json
{
  "hosting": {
    "rewrites": [
      {
        "source": "/api/**",
        "function": "minha-funcao"  // Cloud Function
      },
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

---

## 📊 Fluxo de Deploy

```
firebase deploy
       ↓
Lê firebase.json
       ↓
Identifica projeto padrão
       ↓
Deploy de firestore.rules
       ↓
Upload da pasta "public"
       ↓
Configura rewrites
       ↓
✅ Pronto!
```

---

## ⚙️ Comandos Relacionados

```bash
# Fazer deploy de tudo
firebase deploy

# Deploy apenas de Firestore
firebase deploy --only firestore

# Deploy apenas de Hosting
firebase deploy --only hosting

# Deploy específico
firebase deploy --only firestore:rules,hosting

# Ver status
firebase status

# Listar projetos
firebase projects:list

# Usar outro projeto
firebase use seu-projeto-id
```

---

## 🔗 Referências

- [Firebase JSON Schema](https://firebase.google.com/docs/reference/firebase-cli)
- [Firestore Rules Guide](https://firebase.google.com/docs/firestore/security/start)
- [Firebase Hosting](https://firebase.google.com/docs/hosting)

---

## 💡 Dicas Importantes

✅ **Faça isso:**
- Sempre preencha `projects.default`
- Coloque `.env` em `ignore`
- Use `rewrites` para SPAs
- Teste `firestore.rules` antes de fazer deploy

❌ **Nunca faça:**
- Deixe credenciais em `public/`
- Faça upload de `node_modules`
- Mude `public` sem atualizar arquivos
- Confie apenas em `.gitignore`
