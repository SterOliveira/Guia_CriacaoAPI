# 📘 Documentação da API de Teste com Express.js

## 📌 Visão Geral

Esta API foi criada como um exemplo simples usando **Node.js** com o framework **Express.js**. Ela permite:

- Verificar se a API está funcionando.
- Listar usuários fictícios.
- Simular a criação de novos usuários.

---

## 📁 Estrutura do Projeto
minha-api-teste/
│
├── index.js # Código principal da API
├── package.json # Arquivo de configuração do projeto Node.js
└── node_modules/ # Pacotes instalados (como o Express)

---

## 🚀 Como Executar a API

### ✅ Pré-requisitos

- Ter o **Node.js** instalado ([https://nodejs.org/](https://nodejs.org/))

### ▶️ Instalação do Projeto

Abra o terminal e execute:

```bash
npm init -y             # Cria o arquivo package.json
npm install express     # Instala o Express
```
## 🔧 Código Principal (index.js)

```javascript
const express = require('express');
const app = express();
const PORT = 3000;

app.use(express.json()); // Permite receber JSON no corpo da requisição

// Rota GET simples
app.get('/', (req, res) => {
  res.send('API está funcionando!');
});

// Rota GET com dados fictícios
app.get('/usuarios', (req, res) => {
  const usuarios = [
    { id: 1, nome: 'Ana' },
    { id: 2, nome: 'João' },
  ];
  res.json(usuarios);
});

// Rota POST para criar usuário (simulado)
app.post('/usuarios', (req, res) => {
  const novoUsuario = req.body;
  res.status(201).json({ mensagem: 'Usuário criado com sucesso', dados: novoUsuario });
});

// Iniciar servidor
app.listen(PORT, () => {
  console.log(`Servidor rodando em http://localhost:${PORT}`);
});
```
