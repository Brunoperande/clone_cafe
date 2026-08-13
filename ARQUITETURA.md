# 🏗️ Arquitetura do Projeto Ouro Negro

## 📊 Diagrama de Arquitetura

```
┌─────────────────────────────────────────────────────────────┐
│                    USUÁRIO FINAL                             │
│              (Navegador - Cliente)                           │
└────────────────────────┬────────────────────────────────────┘
                         │
                    📱 HTML5/CSS/JS
                         │
         ┌───────────────┴────────────────┐
         │                                │
    ┌────▼──────────┐          ┌─────────▼────────┐
    │  index.html   │          │   main.js        │
    │ Estrutura HTML│          │  Lógica Frontend │
    └────┬──────────┘          └─────────┬────────┘
         │                              │
         └──────────────┬───────────────┘
                        │
                ┌───────▼────────┐
                │ style.css      │
                │ Estilos CSS    │
                └────────────────┘
                        │
    ┌───────────────────┴────────────────────┐
    │                                         │
    │        FIREBASE SDK (Cliente)          │
    │                                         │
    └───────────────────┬────────────────────┘
         │              │             │
    ┌────▼───┐      ┌───▼───┐   ┌────▼────────┐
    │  Auth  │      │Storage│   │  Firestore  │
    │        │      │       │   │  Database   │
    └────────┘      └───────┘   └────┬────────┘
                                      │
                    ┌─────────────────┴──────────────────┐
                    │                                    │
              ┌─────▼──────┐              ┌──────────────▼──┐
              │ Firestore  │              │ Firestore Rules │
              │  Database  │              │   (Segurança)   │
              │            │              │                 │
              │ - usuarios │              │ Verifica:       │
              │ - produtos │              │ - Autenticação  │
              │ - pedidos  │              │ - Permissões    │
              │ - cats.    │              │ - Validações    │
              └────────────┘              └─────────────────┘
```

---

## 🔄 Fluxos Principais

### 1️⃣ Fluxo de Visualização de Produtos

```
Usuário acessa index.html
        ↓
main.js carrega
        ↓
Chama Firebase SDK
        ↓
Firestore.rules verifica (leitura de produtos)
        ↓
✅ Produtos são públicos → Acesso permitido
        ↓
Dados carregados em main.js
        ↓
Renderizados em index.html via JavaScript
        ↓
Usuário vê produtos na página
```

### 2️⃣ Fluxo de Autenticação

```
Usuário clica em "Login"
        ↓
main.js abre formulário de login
        ↓
Usuário digita email/senha
        ↓
Firebase Auth valida credenciais
        ↓
✅ Válido → Gera token JWT
❌ Inválido → Mostra erro
        ↓
main.js armazena token (localStorage)
        ↓
Usuário pode agora fazer pedidos
```

### 3️⃣ Fluxo de Pedido

```
Usuário adiciona itens ao carrinho
        ↓
main.js salva no localStorage
        ↓
Usuário clica "Finalizar Pedido"
        ↓
main.js verifica autenticação
        ↓
❌ Não autenticado → Redireciona para login
✅ Autenticado → Continua
        ↓
Cria documento em Firestore /pedidos/
        ↓
firestore.rules verifica permissão
        ↓
✅ Usuário é dono → Documento criado
        ↓
main.js mostra confirmação
        ↓
Pedido salvo no banco de dados
```

---

## 📁 Estrutura de Pastas

```
cafe_clone/
│
├── 📄 index.html              # Entrada principal
├── 🎨 style.css               # Estilos
├── ⚙️ main.js                # Lógica principal
├── 🖼️ *.png                  # Imagens
│
├── 📖 COMO_FUNCIONA.md        # Guia de funcionamento
├── 📖 DOCUMENTACAO.md         # Documentação completa
├── 🏗️ ARQUITETURA.md         # Este arquivo
│
└── 🔒 seguro/                 # CONFIGURAÇÕES SENSÍVEIS
    ├── 🔐 .env                # Credenciais (NÃO versionar)
    ├── 📋 .env.example        # Template
    ├── 📝 firebase.json       # Config Firebase
    ├── 📜 firestore.rules     # Regras de segurança
    ├── 📖 README.md           # Setup Firebase
    ├── 📋 FIREBASE_JSON_EXPLICADO.md
    └── 📊 ARQUITETURA.md      # Este arquivo
```

---

## 🔐 Camadas de Segurança

```
┌─────────────────────────────────────────────────┐
│      CAMADA 1: AUTENTICAÇÃO                     │
│  Verifica se usuário está logado                │
│  (Firebase Auth)                                │
└─────────────┬───────────────────────────────────┘
              │
┌─────────────▼───────────────────────────────────┐
│      CAMADA 2: AUTORIZAÇÃO                      │
│  Verifica o que usuário pode fazer              │
│  (Firestore Rules)                              │
└─────────────┬───────────────────────────────────┘
              │
┌─────────────▼───────────────────────────────────┐
│      CAMADA 3: VALIDAÇÃO                        │
│  Verifica se dados são válidos                  │
│  (Firestore Rules + Cloud Functions)           │
└──────────────────────────────────────────────────┘
```

---

## 💾 Fluxo de Dados Detalhado

### Leitura de Produtos
```javascript
// main.js
const produtos = await db.collection('produtos')
  .where('ativo', '==', true)
  .get();

↓

// firestore.rules verifica
match /produtos/{doc=**} {
  allow read: if true;  // ✅ Público
}

↓

// Dados retornam para main.js
// HTML é atualizado
```

### Criação de Pedido
```javascript
// main.js
const pedido = await db.collection('pedidos').add({
  userId: auth.currentUser.uid,
  dataPedido: new Date(),
  itens: carrinho,
  total: calculoTotal()
});

↓

// firestore.rules verifica
match /pedidos/{pedidoId} {
  allow create: if request.auth != null && 
                   request.resource.data.userId == request.auth.uid;
}

↓

// ✅ Se usuário ID bate com userId → Criado
// ❌ Se não bate → Negado
```

---

## 🎯 Responsabilidades de Cada Arquivo

| Arquivo | Responsabilidade |
|---------|-----------------|
| **index.html** | Estrutura HTML da página |
| **style.css** | Estilos e layout |
| **main.js** | Lógica de negócio e interações |
| **firebase.json** | Configuração de deploy |
| **firestore.rules** | Segurança de acesso aos dados |
| **.env** | Credenciais confidenciais |
| **COMO_FUNCIONA.md** | Guia de funcionamento |

---

## 🚀 Ciclo de Vida de uma Request

```
1. CLIENTE (Browser)
   ├─ Usuário faz ação
   ├─ JavaScript captura evento
   └─ Chama Firebase SDK

2. TRANSPORT (Internet)
   └─ HTTPS encrypta comunicação

3. FIREBASE (Backend)
   ├─ Recebe request
   ├─ Verifica autenticação
   ├─ Verifica firestore.rules
   └─ Se OK → Processa dados

4. BANCO DE DADOS
   ├─ Operação executada
   └─ Resposta preparada

5. RESPOSTA (De volta)
   ├─ Firebase envia dados
   ├─ JavaScript recebe
   └─ Atualiza página
```

---

## 📊 Tipos de Dados no Firestore

```
┌────────────────────────────────────────┐
│         COLLECTIONS (Tabelas)          │
├────────────────────────────────────────┤
│                                        │
│  usuarios/                             │
│  ├─ user1 {Document}                  │
│  └─ user2 {Document}                  │
│                                        │
│  produtos/                             │
│  ├─ prod1 {Document}                  │
│  └─ prod2 {Document}                  │
│                                        │
│  pedidos/                              │
│  ├─ ped1 {Document}                   │
│  │   └─ itens/ {Subcollection}        │
│  └─ ped2 {Document}                   │
│                                        │
└────────────────────────────────────────┘
```

---

## 🔍 Fluxo de Validação

```
Dados chegam do Cliente
        ↓
├─ Firestore.rules valida
│  ├─ Autenticação ✓
│  ├─ Permissão ✓
│  ├─ Tipo de dados ✓
│  └─ Schema ✓
        ↓
├─ Cloud Functions validam (se existirem)
│  ├─ Lógica de negócio ✓
│  └─ Consistência ✓
        ↓
└─ Banco de dados executa
   └─ Dados salvos ✓
```

---

## 🔗 Conexões Principais

```
HTML ←→ CSS
 ↓
 JavaScript (main.js)
 ↓
 Firebase SDK
 ↓
 ├─ Auth (Autenticação)
 ├─ Firestore (Banco de dados)
 └─ Storage (Arquivos)
 ↓
 firestore.rules (Segurança)
```

---

## 📈 Escalabilidade

### Atualmente
- ✅ Um único index.html
- ✅ Um único style.css
- ✅ Um único main.js

### Próximos Passos
- 📦 Modularizar com modules ES6
- ⚙️ Adicionar build tool (Webpack/Vite)
- 📱 Melhorar responsividade
- 🎯 Adicionar PWA (Progressive Web App)

---

## 🛠️ Stack Tecnológico

```
Frontend:
  - HTML5
  - CSS3 (Flexbox/Grid)
  - JavaScript ES6+
  - Firebase SDK

Backend:
  - Firestore Database
  - Firebase Authentication
  - Firebase Hosting

Segurança:
  - Firestore Rules Language
  - HTTPS/SSL
  - Authentication tokens (JWT)
```

---

## 📞 Referências

- [Firebase Architecture](https://firebase.google.com/docs/guides/app-architecture)
- [Firestore Best Practices](https://firebase.google.com/docs/firestore/best-practices)
- [Security Rules](https://firebase.google.com/docs/firestore/security/start)
