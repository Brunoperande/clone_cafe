# 📖 Como o Projeto Funciona

## 🎯 Visão Geral

**Ouro Negro** é um site de café artesanal e gourmet com integração Firebase para gerenciar dados de forma segura e escalável.

## 🏗️ Estrutura do Projeto

```
cafe_clone/
├── index.html              # Página principal do site
├── style.css              # Estilos CSS
├── main.js                # JavaScript principal
├── test.html              # Página de testes
├── plano-escalabilidade.jsx  # Plano de arquitetura
├── *.png                  # Imagens do projeto (produtos, logo, etc)
├── seguro/                # 🔒 Pasta de configurações sensíveis
│   ├── .env              # Variáveis de ambiente (NÃO commitar)
│   ├── .env.example      # Modelo para .env
│   ├── firebase.json     # Config do Firebase
│   ├── firestore.rules   # Regras de segurança do Firestore
│   └── README.md         # Documentação da pasta seguro
└── README.md             # Este arquivo
```

## 🚀 Como Funciona

### 1. **Frontend (Interface)**
- **index.html**: Página principal com catálogo de produtos
- **style.css**: Estilos da página (layout, cores, responsividade)
- **main.js**: Lógica JavaScript (interações, carrinho, pedidos)

### 2. **Backend (Firebase)**
- **Firestore Database**: Armazena dados de produtos, pedidos e usuários
- **Firebase Authentication**: Sistema de login dos usuários
- **Firebase Hosting**: Hospedagem da página web

### 3. **Segurança (Pasta seguro/)**
```
┌─────────────────────────────────────────┐
│     Usuário acessa o site web           │
└─────────────────┬───────────────────────┘
                  │
        ┌─────────▼──────────┐
        │  index.html (JS)   │
        └─────────┬──────────┘
                  │
        ┌─────────▼──────────────────┐
        │  Verifica autenticação     │
        │  (Firebase Auth)           │
        └─────────┬──────────────────┘
                  │
        ┌─────────▼──────────────────┐
        │  Firestore Rules verificam │
        │  permissões (segurança)    │
        └─────────┬──────────────────┘
                  │
        ┌─────────▼──────────────────┐
        │  Acessa dados do Firestore │
        │  (Produtos, Pedidos, etc)  │
        └─────────────────────────────┘
```

## 🔐 Segurança

### .env (Variáveis de Ambiente)
Contém credenciais confidenciais:
```
FIREBASE_API_KEY=xxxxx
FIREBASE_AUTH_DOMAIN=seu-projeto.firebaseapp.com
FIREBASE_PROJECT_ID=seu-projeto-id
...
```

### firestore.rules
Regras que definem quem pode acessar os dados:
- ✅ Usuários autenticados podem ler/escrever seus próprios dados
- ✅ Todos podem ver produtos públicos
- ❌ Dados sensíveis são protegidos

## 📦 Fluxo de Funcionalidades

### 1. Visualizar Produtos
```
1. Usuário acessa o site
2. main.js carrega produtos do Firestore
3. Produtos são exibidos na página
```

### 2. Fazer um Pedido
```
1. Usuário adiciona itens ao carrinho
2. Clica em "Finalizar Pedido"
3. Sistema verifica autenticação
4. Pedido é salvo no Firestore
5. Confirmação enviada para o usuário
```

### 3. Gerenciar Conta
```
1. Usuário faz login
2. Firebase armazena sessão
3. Dados do usuário carregados do Firestore
4. Pode visualizar pedidos anteriores
```

## 🛠️ Variáveis Importantes

### .env (Obrigatório preencher)
```
FIREBASE_API_KEY=         # API Key do Firebase
FIREBASE_PROJECT_ID=      # ID do projeto
FIREBASE_AUTH_DOMAIN=     # Domínio de autenticação
FIREBASE_STORAGE_BUCKET=  # Bucket de armazenamento
APP_URL=                  # URL da aplicação
```

### firebase.json
Configura o Firebase com:
- 📁 Pasta pública (hosting)
- 🔗 Redirecionamentos (rewrite)
- 📊 Índices do Firestore
- 📜 Regras de segurança

## 🚀 Como Executar

### 1. Setup Inicial
```bash
# Clone o repositório
git clone https://github.com/Brunonunes06/clone_cafe.git

# Entre na pasta
cd clone_cafe

# Configure as variáveis de ambiente
cd seguro
cp .env.example .env
# Edite .env com suas credenciais do Firebase
```

### 2. Instalar Firebase CLI
```bash
npm install -g firebase-tools
firebase login
```

### 3. Implantar Regras do Firestore
```bash
cd seguro
firebase deploy --only firestore:rules
```

### 4. Executar Localmente
```bash
# Inicie servidor local (se disponível)
firebase serve

# Ou abra index.html no navegador
```

## 📊 Estrutura do Firestore

### Coleções Principais

#### 1. `usuarios`
```json
{
  "uid": "user123",
  "nome": "João Silva",
  "email": "joao@email.com",
  "telefone": "11999999999",
  "dataCriacao": "2024-01-15",
  "role": "cliente"
}
```

#### 2. `produtos`
```json
{
  "id": "prod001",
  "nome": "Café Premium",
  "descricao": "Grãos especiais do Brasil",
  "preco": 35.90,
  "imagem": "cafe.png",
  "categoria": "aromatico",
  "estoque": 100
}
```

#### 3. `pedidos`
```json
{
  "id": "ped001",
  "userId": "user123",
  "datasPedido": "2024-01-15",
  "status": "confirmado",
  "total": 99.90,
  "itens": [
    {
      "produtoId": "prod001",
      "quantidade": 2,
      "preco": 35.90
    }
  ]
}
```

## 🔍 Fluxo de Autenticação

```
┌─────────────┐
│ Visitante   │ ──> Pode ver produtos
└─────────────┘

┌─────────────┐
│ Login       │ ──> Firebase Auth valida credenciais
└─────────────┘

┌──────────────┐
│ Autenticado  │ ──> Acessa Firestore com permissões
└──────────────┘

┌──────────────┐
│ Admin/Staff  │ ──> Permissões elevadas
└──────────────┘
```

## 📱 Responsividade

O projeto é responsivo para:
- 📱 Celular (< 600px)
- 📱 Tablet (600px - 1024px)
- 🖥️ Desktop (> 1024px)

Estilos em `style.css` usam media queries para adaptar layout.

## 🔄 Ciclo de Desenvolvimento

1. **Modificar código** → index.html, style.css, main.js
2. **Testar localmente** → Abrir no navegador
3. **Atualizar Firestore** → Modificar firestore.rules
4. **Deploy** → `git push` e `firebase deploy`

## 🆘 Troubleshooting

### "Erro de autenticação"
→ Verifique `.env` com credenciais corretas

### "Produtos não carregam"
→ Verifique `firestore.rules` e se banco de dados tem dados

### "Deploy falha"
→ Execute `firebase login` e `firebase init` novamente

## 📞 Suporte

Para mais informações, veja:
- `seguro/README.md` - Configuração do Firebase
- `README.md` - Informações do projeto
- `firebase.json` - Configurações de deploy
