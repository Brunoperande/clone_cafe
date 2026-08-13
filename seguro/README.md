# 🔒 Configuração de Segurança - Firebase

Este diretório contém as configurações de segurança e Firebase do projeto Ouro Negro.

## 📁 Arquivos Importantes

| Arquivo | Descrição |
|---------|-----------|
| `.env` | 🚫 Credenciais sensíveis (NÃO commitar) |
| `.env.example` | ✅ Template para copiar |
| `.gitignore` | Arquivos a ignorar no Git |
| `firebase.json` | Configurações do Firebase |
| `firestore.rules` | Regras de segurança do Firestore |

---

## 🚀 Setup Inicial

### 1. Criar projeto no Firebase
```bash
# Acesse https://console.firebase.google.com
# 1. Clique em "Novo Projeto"
# 2. Digite "cafe" como nome
# 3. Aceite as condições
# 4. Crie o projeto
```

### 2. Configurar .env
```bash
# Copie o arquivo de exemplo
cp .env.example .env

# Abra .env e preencha com as credenciais:
# FIREBASE_API_KEY=sua-api-key-aqui
# FIREBASE_PROJECT_ID=seu-project-id
# ... etc
```

### 3. Instalar Firebase CLI
```bash
# Install globalmente
npm install -g firebase-tools

# Faça login
firebase login

# Inicialize o projeto
firebase init firestore
```

### 4. Deployr Regras de Segurança
```bash
# Deploy apenas as regras
firebase deploy --only firestore:rules

# Ou deploy tudo
firebase deploy
```

---

## 🔐 Regras de Segurança (firestore.rules)

### Como Funciona?

As regras do Firestore definem **quem** pode **fazer o quê** com os dados:

```
┌─────────────────────────────────────────────────┐
│  Usuário tenta acessar um documento             │
└──────────────────┬──────────────────────────────┘
                   │
         ┌─────────▼──────────┐
         │ Firestore verifica │
         │ as rules           │
         └─────────┬──────────┘
                   │
        ┌──────────▼──────────┐
        │ Regra permite?      │
        └──┬──────────────┬───┘
           │              │
      ✅ SIM          ❌ NÃO
           │              │
      Acesso OK    Acesso Negado
```

### Estrutura das Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Regras são verificadas aqui
    match /caminho/{documento} {
      allow read;    // Permitir leitura
      allow write;   // Permitir escrita
    }
  }
}
```

### Exemplos de Rules

#### 1️⃣ Qualquer um pode ler
```javascript
match /produtos/{doc=**} {
  allow read: if true;           // ✅ Todos podem ver
  allow write: if false;         // ❌ Ninguém pode editar
}
```

#### 2️⃣ Apenas autenticados podem escrever
```javascript
match /usuarios/{userId} {
  allow read, write: if request.auth != null;  // ✅ Autenticado
}
```

#### 3️⃣ Cada usuário vê só seus dados
```javascript
match /usuarios/{userId} {
  allow read, write: if request.auth.uid == userId;  // ✅ Seu próprio ID
}
```

#### 4️⃣ Apenas admins podem editar
```javascript
match /produtos/{doc=**} {
  allow read: if true;  // Todos leem
  allow write: if hasRole('admin');  // Só admins escrevem
}

function hasRole(role) {
  return get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == role;
}
```

### Regras do Projeto

```
📖 LEITURA (read)
├── Produtos: ✅ Todos podem ver
├── Pedidos: ✅ Só o dono pode ver
└── Usuários: ✅ Só dados próprios

✏️ ESCRITA (write)
├── Produtos: ❌ Apenas admin
├── Pedidos: ✅ Criar novo, atualizar próprio
└── Usuários: ✅ Atualizar dados próprios
```

---

## 📝 Variáveis de Ambiente (.env)

### O que é .env?

Arquivo que armazena **valores sensíveis** que não devem ir para o Git:
- Chaves de API
- IDs de projeto
- URLs confidenciais

### Template (.env.example)

```bash
# Firebase API
FIREBASE_API_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
FIREBASE_AUTH_DOMAIN=seu-projeto.firebaseapp.com
FIREBASE_PROJECT_ID=seu-projeto-12345
FIREBASE_STORAGE_BUCKET=seu-projeto.appspot.com
FIREBASE_MESSAGING_SENDER_ID=123456789
FIREBASE_APP_ID=1:123456789:web:abcdef123456
FIREBASE_MEASUREMENT_ID=G-XXXXXXXXXX

# App URL
APP_URL=http://localhost:3000
APP_ENV=development
```

### Como Preencher?

1. Acesse [Firebase Console](https://console.firebase.google.com)
2. Clique no seu projeto
3. Vá em "Configurações do Projeto"
4. Role até "Seus aplicativos" → "Web"
5. Copie as credenciais
6. Cole em `.env`

---

## ⚠️ Segurança - Boas Práticas

### ✅ Faça Isso
```
✅ Adicione .env ao .gitignore
✅ Use variáveis de ambiente
✅ Teste as regras antes de fazer deploy
✅ Monitore acessos ao banco de dados
✅ Use autenticação forte
```

### ❌ Nunca Faça Isso
```
❌ Commite .env no Git
❌ Compartilhe credenciais por email
❌ Use a mesma chave em múltiplos ambientes
❌ Coloque credenciais no código-fonte
❌ Confie apenas em regras do lado do cliente
```

---

## 🔑 Onde Encontrar Credenciais

### Firebase Console
1. [https://console.firebase.google.com](https://console.firebase.google.com)
2. Clique no seu projeto
3. ⚙️ Engrenagem → "Configurações do Projeto"
4. Aba "Seu App" → Selecione "Web"
5. Copie `firebaseConfig`

### firebase.json

Arquivo que configura o Firebase CLI:
- Qual banco de dados usar
- Onde estão as regras (firestore.rules)
- Configuração de hosting

---

## 🚀 Comandos Úteis

```bash
# Fazer login
firebase login

# Verificar conectividade
firebase projects:list

# Deploy de regras
firebase deploy --only firestore:rules

# Deploy completo
firebase deploy

# Testar regras localmente
firebase emulators:start

# Ver logs em tempo real
firebase functions:log
```

---

## 🐛 Troubleshooting

### ❌ Erro: "Permission denied"
**Problema**: Regra não permite acesso
**Solução**: Verifique `firestore.rules` ou autenticação

### ❌ Erro: ".env not found"
**Problema**: Arquivo de configuração faltando
**Solução**: Execute `cp .env.example .env` e preencha

### ❌ Erro: "Invalid API Key"
**Problema**: Credenciais incorretas
**Solução**: Verifique e copie novamente do Firebase Console

### ❌ Regras não fazem efeito
**Problema**: Deploy não foi executado
**Solução**: Execute `firebase deploy --only firestore:rules`

---

## 📊 Estrutura do Firestore

### Collections Criadas

```
📁 Firestore Database
├── 📂 usuarios/
│   └── {userId}
│       ├── nome: string
│       ├── email: string
│       ├── telefone: string
│       ├── role: admin|cliente
│       └── dataCriacao: timestamp
│
├── 📂 produtos/
│   └── {produtoId}
│       ├── nome: string
│       ├── descricao: string
│       ├── preco: number
│       ├── categoria: string
│       ├── imagem: string
│       ├── estoque: number
│       └── ativo: boolean
│
├── 📂 pedidos/
│   └── {pedidoId}
│       ├── userId: string
│       ├── dataPedido: timestamp
│       ├── status: pendente|confirmado|enviado|entregue
│       ├── total: number
│       ├── itens: array
│       └── endereco: string
│
└── 📂 categorias/
    └── {categoriaId}
        ├── nome: string
        ├── descricao: string
        └── imagem: string
```

---

## 🔗 Recursos

- [Firebase Docs](https://firebase.google.com/docs)
- [Firestore Security Rules](https://firebase.google.com/docs/firestore/security/start)
- [Firebase CLI Docs](https://firebase.google.com/docs/cli)

---

## 📞 Suporte

Dúvidas? Veja:
- 📖 [COMO_FUNCIONA.md](../COMO_FUNCIONA.md)
- 📖 [DOCUMENTACAO.md](../DOCUMENTACAO.md)
- [Firebase Community](https://stackoverflow.com/questions/tagged/firebase)
