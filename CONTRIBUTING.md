# Como Contribuir

Obrigado pelo interesse em contribuir para a API Aberta! 🇵🇹

## Formas de contribuir

### 🔌 Criar um conector

A forma mais impactante de ajudar. Cada conector traz novos dados para a plataforma.

1. Verifica se a fonte ainda não tem conector em [fontes-de-dados.md](docs/fontes-de-dados.md)
2. Abre uma issue com o label `novo-conector` para discutir
3. Faz fork do repo `connectors`
4. Implementa seguindo a [guia de conectores](docs/conectores.md)
5. Abre um Pull Request

### 🐛 Reportar um bug

1. Verifica se o bug já foi reportado nas [issues](https://github.com/apiaberta/apiaberta/issues)
2. Usa o template "Bug Report"
3. Inclui passos para reproduzir, comportamento esperado vs actual

### 💡 Sugerir uma funcionalidade

1. Abre uma issue com o label `sugestão`
2. Descreve o caso de uso e o valor que traz

### 📝 Melhorar a documentação

Erros, ambiguidades ou lacunas na documentação são bugs. PRs para docs são sempre bem-vindos.

---

## Setup de desenvolvimento

```bash
# Clonar
git clone https://github.com/apiaberta/api.git
cd api

# Instalar dependências
npm install

# Variáveis de ambiente
cp .env.example .env
# Editar .env com as tuas configurações

# Correr localmente
npm run dev
```

Requisitos: Node.js 22+, MongoDB 7+

---

## Convenções

- **Commits:** mensagens em português, imperativo (`Adiciona endpoint X`, `Corrige bug Y`)
- **Branches:** `feat/nome`, `fix/nome`, `docs/nome`
- **PRs:** descrição clara do que muda e porquê
- **Código:** ESLint + Prettier (configuração no repo)

---

## Código de Conduta

Este projecto segue o [Código de Conduta](CODE_OF_CONDUCT.md). Respeito e colaboração acima de tudo.
