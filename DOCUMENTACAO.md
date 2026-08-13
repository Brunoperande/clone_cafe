# 🏪 Ouro Negro - Café Artesanal & Gourmet

## 📝 Sobre o Projeto

**Ouro Negro** é um site modern e responsivo de uma cafeteria artesanal e gourmet. O projeto integra **Firebase** para gerenciar dados de clientes, produtos e pedidos de forma segura.

## ✨ Características

- ☕ Catálogo completo de produtos de café
- 🛒 Carrinho de compras funcional
- 👤 Sistema de autenticação com Firebase
- 💾 Armazenamento de pedidos no Firestore
- 🔒 Segurança com regras do Firestore
- 📱 Design responsivo (mobile, tablet, desktop)
- 🎨 Interface atraente e intuitiva

## 🗂️ Estrutura do Projeto

```
📦 cafe_clone
├── 📄 index.html                    # Página principal
├── 🎨 style.css                    # Estilos
├── ⚙️ main.js                      # Lógica JavaScript
├── 🏗️ plano-escalabilidade.jsx    # Arquitetura
├── 📖 COMO_FUNCIONA.md            # Guia de funcionamento
├── 🖼️ *.png                       # Imagens (logo, produtos)
│
└── 🔒 seguro/                      # CONFIGURAÇÕES SENSÍVEIS
    ├── .env                       # Variáveis (NÃO COMMITAR!)
    ├── .env.example              # Modelo
    ├── firebase.json             # Config Firebase
    ├── firestore.rules           # Regras de segurança
    └── README.md                 # Setup Firebase
```

## 🚀 Início Rápido

### 1️⃣ Clonar Repositório
```bash
git clone https://github.com/Brunonunes06/clone_cafe.git
cd clone_cafe
```

### 2️⃣ Configurar Variáveis de Ambiente
```bash
cd seguro
cp .env.example .env
# Edite .env com suas credenciais do Firebase
```

### 3️⃣ Instalar Firebase CLI
```bash
npm install -g firebase-tools
firebase login
```

### 4️⃣ Deployr Regras de Segurança
```bash
cd seguro
firebase deploy --only firestore:rules
```

### 5️⃣ Abrir no Navegador
```bash
# Opção 1: Usar Firebase Hosting
firebase serve

# Opção 2: Abrir arquivo diretamente
# Clique em index.html para abrir no navegador
```

## 🔑 Chaves do Firebase

Para funcionar, você precisa:

1. Criar projeto no [Firebase Console](https://console.firebase.google.com)
2. Habilitar Firestore Database
3. Habilitar Authentication (Email/Password, Google)
4. Copiar credenciais para `seguro/.env`

### Credenciais Necessárias
```
FIREBASE_API_KEY
FIREBASE_AUTH_DOMAIN
FIREBASE_PROJECT_ID
FIREBASE_STORAGE_BUCKET
FIREBASE_MESSAGING_SENDER_ID
FIREBASE_APP_ID
FIREBASE_MEASUREMENT_ID
```

## 📚 Documentação

- [📖 Como Funciona](COMO_FUNCIONA.md) - Guia detalhado do projeto
- [🔒 Setup Firebase](seguro/README.md) - Configuração de segurança
- [📋 Firestore Rules](seguro/firestore.rules) - Regras de acesso

## 🏗️ Tecnologias

- 🌐 **Frontend**: HTML5, CSS3, JavaScript vanilla
- ☁️ **Backend**: Firebase (Firestore, Auth, Hosting)
- 🔐 **Segurança**: Firestore Rules, JWT tokens
- 📱 **Responsivo**: Media queries CSS

## 📊 Fluxo de Dados

```
Usuário (Browser)
    ↓
  index.html (UI)
    ↓
  main.js (Lógica)
    ↓
Firebase SDK
    ↓
  [Authentication] [Firestore Database]
```

## 🔐 Segurança

- ✅ Credenciais em `.env` (não versionadas)
- ✅ Regras do Firestore verificam permissões
- ✅ Autenticação obrigatória para pedidos
- ✅ Dados do usuário isolados

Veja [seguro/firestore.rules](seguro/firestore.rules) para detalhes.

## 🎯 Funcionalidades Principais

### 1. Catálogo de Produtos
```
- Exibição de produtos com imagem, preço e descrição
- Filtros por categoria
- Busca de produtos
```

### 2. Carrinho de Compras
```
- Adicionar/remover itens
- Atualizar quantidades
- Calcular total
- Persistência no localStorage
```

### 3. Sistema de Pedidos
```
- Criar novo pedido
- Histórico de pedidos
- Status do pedido
- Integração Firestore
```

### 4. Autenticação
```
- Login/Signup
- Recuperação de senha
- Verificação de email
- Perfil do usuário
```

## 💾 Estrutura do Firestore

### Collections
- **usuarios** - Dados dos clientes
- **produtos** - Catálogo de produtos
- **pedidos** - Histórico de compras
- **categorias** - Categorias de produtos

## 🖥️ Desenvolvimento

### Abrir em Desenvolvimento
```bash
cd cafe_clone
# Abrir em editor (VS Code recomendado)
code .

# Abrir index.html no navegador (F5 ou Live Server)
```

### Estrutura de Arquivos
- `index.html` - Markup HTML
- `style.css` - Estilos globais
- `main.js` - Funções principais
- `test.html` - Testes

## 🚀 Deploy

### Deploy Manual
```bash
firebase deploy
```

### Deploy Automático
Configure GitHub Actions para deploy automático ao fazer push na branch `main`.

## 📱 Responsividade

| Dispositivo | Breakpoint | Layout |
|-------------|-----------|--------|
| 📱 Celular | < 600px | Mobile |
| 📱 Tablet | 600px - 1024px | Tablet |
| 🖥️ Desktop | > 1024px | Desktop |

## 🐛 Problemas Comuns

### ❌ "Erro ao carregar produtos"
**Solução**: Verifique se Firestore tem permissão de leitura em `firestore.rules`

### ❌ "Não conseguir fazer login"
**Solução**: Verifique credenciais do Firebase em `.env`

### ❌ "Página em branco"
**Solução**: Abra console (F12) e verifique erros JavaScript

## 📚 Recursos

- [Firebase Docs](https://firebase.google.com/docs)
- [JavaScript MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [CSS Grid/Flexbox](https://css-tricks.com/)

## 📝 Licença

Este projeto é de código aberto. Veja `LICENSE` para detalhes.

## 👨‍💻 Autor

**Bruno Nunes**
- GitHub: [@Brunonunes06](https://github.com/Brunonunes06)
- Projeto: [clone_cafe](https://github.com/Brunonunes06/clone_cafe)

## 🤝 Contribuições

Contribuições são bem-vindas! 

1. Fork o projeto
2. Crie sua feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📞 Suporte

Encontrou um bug? Abra uma [Issue](https://github.com/Brunonunes06/clone_cafe/issues)!

---

**⭐ Se gostou do projeto, deixe uma star!**
