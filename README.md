# Portaria UNITEC - Sistema de Controle de Portaria

## 📋 Sobre o Projeto

Sistema web para gerenciamento de portaria, controlando entrada e saída de visitantes, veículos e motoristas.

## ✨ Funcionalidades

### Para Porteiros
- 👥 Registro de visitantes (entrada/saída)
- 🚛 Controle de entrada/saída de veículos
- 🚗 Cadastro e gerenciamento de motoristas
- 📊 Dashboard com resumo de atividades
- 🔍 Busca avançada de registros

### Para Administradores
- 👤 Gerenciamento de usuários (porteiros, supervisores, admins)
- 📇 Agenda de contatos
- 📈 Relatórios e estatísticas
- 📋 Logs de auditoria
- 🗑️ Ferramentas de manutenção

### Para Supervisores
- 📊 Visualização de todos os dados
- 📈 Acesso a relatórios
- 🔍 Consulta de registros
- ❌ **Sem** permissão para criar/editar/deletar

## 🔒 Segurança

### ✅ Proteções Implementadas

1. **Proteção XSS (Cross-Site Scripting)**
   - Escape HTML em todas as saídas
   - 80% de cobertura em áreas críticas
   - Previne injeção de scripts maliciosos

2. **Validação e Sanitização de Entrada**
   - Limites de comprimento
   - Remoção de caracteres de controle
   - Filtro de null bytes
   - 90% de cobertura

3. **Proteção contra Brute Force**
   - Limite de 5 tentativas por email
   - Bloqueio de 15 minutos
   - Persistente (não contorna com refresh)

4. **Gerenciamento de Sessão**
   - Timeout de 30 minutos de inatividade
   - Renovação automática em atividade
   - Logout forçado após timeout

5. **Proteção de Dados**
   - Senhas não salvas no localStorage
   - Mensagens de erro genéricas
   - Logs de auditoria

### ⚠️ Limitações Conhecidas

1. **Senhas em Texto Plano**
   - Armazenadas sem hash (apenas em memória)
   - **Requer backend** para hash seguro (bcrypt)
   - **NÃO usar em produção** sem backend

2. **Aplicação 100% Frontend**
   - Sem validação server-side
   - Possível manipulação local
   - Backend recomendado para produção

3. **Sem HTTPS Enforcement**
   - Depende da configuração do servidor
   - HTTPS obrigatório para produção

## 🚀 Como Usar

### Instalação

1. Clone o repositório:
```bash
git clone https://github.com/felipehelppc-desk/portaria-unitec.git
cd portaria-unitec
```

2. Abra o arquivo `index.html` no navegador:
```bash
# Windows
start index.html

# Mac
open index.html

# Linux
xdg-open index.html
```

### Usuários Pré-cadastrados

#### Administrador
- **Email:** admin@portaria.com
- **Senha:** admin123

#### Porteiros
1. **Evilásio Garcez**
   - Email: evilasio@portaria.com
   - Senha: 12345

2. **Joveni Reis**
   - Email: joveni@portaria.com
   - Senha: 12345

3. **Luiz Fabiano**
   - Email: luizfabiano@portaria.com
   - Senha: 12345

4. **Simone Ramos**
   - Email: simone@portaria.com
   - Senha: 12345

## 📖 Documentação

### Documentos de Revisão de Código

1. **[CODE_REVIEW.md](CODE_REVIEW.md)** - Análise completa do código
   - 10 categorias de problemas identificados
   - Classificação por severidade
   - Recomendações detalhadas
   - Plano de implementação em 4 fases

2. **[SECURITY_IMPROVEMENTS.md](SECURITY_IMPROVEMENTS.md)** - Melhorias implementadas
   - Todas as correções de segurança explicadas
   - Comparações antes/depois
   - Instruções de teste
   - Métricas de segurança

3. **[REVIEW_FEEDBACK_ADDRESSED.md](REVIEW_FEEDBACK_ADDRESSED.md)** - Respostas ao feedback
   - Análise de cada comentário da revisão
   - Justificativas técnicas
   - Ações tomadas
   - Status de cada item

## 🛠️ Tecnologias

- **Frontend:** HTML5, CSS3, JavaScript (ES6+)
- **Armazenamento:** localStorage
- **Design:** Responsivo (mobile-first)
- **Tema:** Azul profissional

## 📊 Métricas de Qualidade

### Score de Segurança
- **Antes da Revisão:** 2/10
- **Após Melhorias:** 6/10
- **Meta (com backend):** 9/10

### Cobertura de Proteções
- ✅ XSS Protection: 80%
- ✅ Input Validation: 90%
- ✅ Rate Limiting: 100%
- ✅ Session Management: 100%
- ⚠️ Password Security: 40% (sem hash)

### Vulnerabilidades
- **Críticas:** 1 (senhas em texto plano)
- **Altas:** 0
- **Médias:** 2
- **Baixas:** 3

## ⚠️ Avisos Importantes

### ❌ NÃO Usar em Produção
Este sistema **NÃO deve ser usado em produção** sem as seguintes implementações:

1. **Backend com:**
   - Hash de senhas (bcrypt/Argon2)
   - Autenticação JWT
   - Validação server-side
   - Banco de dados seguro

2. **Infraestrutura:**
   - HTTPS/TLS obrigatório
   - Content Security Policy (CSP)
   - Rate limiting no servidor
   - Monitoramento e logs

3. **Conformidade:**
   - Auditoria de segurança profissional
   - Testes de penetração
   - Conformidade LGPD/GDPR

### ✅ Adequado Para
- Ambiente de desenvolvimento
- Ambiente de teste
- Demonstrações internas
- Proof of concept
- Protótipos

## 🔄 Roadmap

### Curto Prazo (1-2 semanas)
- [ ] Completar cobertura XSS (20% restante)
- [ ] Adicionar validação de formato (email, CPF, telefone)
- [ ] Implementar logs de segurança detalhados
- [ ] Criar dashboard de atividades de usuário

### Médio Prazo (1-2 meses)
- [ ] Desenvolver backend API (Node.js/Express)
- [ ] Implementar bcrypt para senhas
- [ ] Adicionar autenticação JWT
- [ ] Configurar banco de dados (PostgreSQL)
- [ ] Implementar HTTPS

### Longo Prazo (3-6 meses)
- [ ] Auditoria de segurança profissional
- [ ] Testes de penetração
- [ ] Implementar 2FA
- [ ] Conformidade LGPD completa
- [ ] Sistema de monitoramento em tempo real

## 🤝 Contribuindo

Este é um projeto interno. Para contribuir:

1. Leia toda a documentação de segurança
2. Siga as diretrizes de código
3. Teste todas as mudanças
4. Documente alterações de segurança
5. Solicite code review antes de merge

## 📝 Changelog

### v1.0.0 (2025-11-22) - Revisão de Segurança
- ✅ Implementado sistema de escape HTML
- ✅ Adicionada sanitização de entrada
- ✅ Implementado rate limiting de login
- ✅ Adicionado timeout de sessão
- ✅ Senhas removidas do localStorage
- ✅ Documentação completa criada
- ✅ Feedback de revisão endereçado

### Versões Anteriores
- v0.14 - Sistema com tema azul
- v0.13 - Adição de supervisores
- v0.12 - Sistema de agenda
- v0.11 - Controle de motoristas

## 📞 Suporte

Para questões de segurança, consulte:
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [MDN Web Security](https://developer.mozilla.org/en-US/docs/Web/Security)

## 📄 Licença

Propriedade de UNITEC. Uso interno apenas.

## 🎯 Status do Projeto

**Desenvolvimento:** ✅ Ativo  
**Segurança:** ⚠️ Melhorado (requer backend para produção)  
**Documentação:** ✅ Completa  
**Testes:** ⚠️ Manuais apenas  
**Produção:** ❌ Não recomendado sem backend  

---

**Última Atualização:** 22 de Novembro de 2025  
**Versão:** 1.0.0  
**Status:** Em Desenvolvimento
