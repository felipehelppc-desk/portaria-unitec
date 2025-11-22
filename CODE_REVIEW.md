# Revisão de Código - Sistema Portaria UNITEC

## Data da Revisão
22 de Novembro de 2025

## Resumo Executivo
Este documento apresenta uma análise abrangente do código do sistema Portaria UNITEC, identificando vulnerabilidades de segurança, problemas de qualidade de código e recomendações de melhorias.

---

## 🔴 PROBLEMAS CRÍTICOS DE SEGURANÇA

### 1. Armazenamento de Senhas em Texto Plano
**Severidade:** CRÍTICA  
**Localização:** Linhas 1873-1877, 2183

**Problema:**
- Senhas armazenadas sem criptografia no código JavaScript
- Comparação direta de senhas em texto plano
- Senhas visíveis no localStorage e na memória do navegador

**Exemplo:**
```javascript
{ id: 1, nome: 'ADMIN', email: 'admin@portaria.com', senha: 'admin123', ... }
if (user.senha === password) // Comparação insegura
```

**Impacto:**
- Qualquer pessoa com acesso ao navegador pode visualizar todas as senhas
- Senhas expostas em dumps de memória
- Violação de boas práticas de segurança (OWASP Top 10)

**Recomendação:**
```javascript
// Implementar hash de senha usando bcrypt ou similar
// Backend necessário para armazenamento seguro
// Nunca armazenar senhas no frontend
```

---

### 2. Vulnerabilidades XSS (Cross-Site Scripting)
**Severidade:** CRÍTICA  
**Localização:** Linhas 2013, 2025, 2284, 2588, 2638, 2764, 3562, 3569, 4161, 4230, 4671

**Problema:**
- Uso extensivo de `innerHTML` com dados não sanitizados
- Entrada do usuário inserida diretamente no DOM via template literals
- Sem função de escape HTML

**Exemplo:**
```javascript
container.innerHTML = `<td>${visitante.nome}</td>`; // Vulnerável a XSS
```

**Impacto:**
- Atacantes podem injetar scripts maliciosos
- Roubo de dados de sessão
- Redirecionamento para sites maliciosos
- Modificação não autorizada do conteúdo

**Cenário de Ataque:**
1. Usuário malicioso registra visitante com nome: `<script>alert('XSS')</script>`
2. Script é executado quando a lista é renderizada
3. Possível roubo de dados ou sessão

---

### 3. Armazenamento Inseguro de Dados Sensíveis
**Severidade:** CRÍTICA  
**Localização:** Linhas 1898-1902, 1932-1936

**Problema:**
- localStorage usado para dados sensíveis (senhas, informações de usuários)
- Dados não criptografados
- Dados persistem após logout
- Acessível via JavaScript em qualquer script na página

**Exemplo:**
```javascript
localStorage.setItem('portaria_usuarios', JSON.stringify(usuarios));
// Incluindo senhas em texto plano!
```

**Impacto:**
- Exposição de dados sensíveis
- Acesso não autorizado por scripts maliciosos
- Violação da LGPD (Lei Geral de Proteção de Dados)
- Dados persistem indefinidamente

---

### 4. Problemas de Autenticação
**Severidade:** ALTA  
**Localização:** Função login() linha 2153

**Problemas:**
- Sem timeout de sessão
- Sem proteção CSRF
- Sem limite de tentativas de login (brute force)
- Senhas padrão fracas ('admin123', '12345')
- Sem validação de força de senha

**Impacto:**
- Possibilidade de ataques de força bruta
- Sessões podem permanecer ativas indefinidamente
- Senhas fracas facilitam invasões

---

## 🟠 PROBLEMAS DE ALTA PRIORIDADE

### 5. Validação de Entrada Insuficiente
**Severidade:** ALTA

**Problemas:**
- Sem sanitização de entrada
- Sem validação de caracteres especiais
- Sem limites de comprimento em alguns campos
- Possível injeção de código

**Exemplo:**
```javascript
const nome = document.getElementById('visitanteNome').value;
// Sem validação ou sanitização
```

**Recomendação:**
```javascript
function sanitizeInput(input) {
    return input.replace(/[<>]/g, '').trim().substring(0, 100);
}
```

---

### 6. Tratamento de Erros Inadequado
**Severidade:** ALTA

**Problemas:**
- Informações sensíveis em console.log
- Mensagens de erro revelam detalhes internos
- Try-catch presente mas erros logados no console

**Exemplos:**
```javascript
console.log('   Email:', email);
console.log('   Senha:', password ? '***' : '(vazia)'); // Ainda revela estrutura
console.log('✅ TODOS OS USUÁRIOS NO SISTEMA:', usuarios.length);
```

**Impacto:**
- Facilita ataques ao revelar estrutura do sistema
- Vazamento de informações sensíveis
- Problemas de produção

---

### 7. Organização do Código
**Severidade:** ALTA

**Problemas:**
- Arquivo único com 4.819 linhas
- HTML, CSS e JavaScript misturados
- Sem separação de responsabilidades
- Difícil manutenção e teste
- Sem versionamento de assets

**Estrutura Atual:**
```
index.html (4819 linhas)
├── <style> (1000+ linhas de CSS)
└── <script> (3000+ linhas de JavaScript)
```

**Estrutura Recomendada:**
```
/src
├── /css
│   ├── main.css
│   ├── components.css
│   └── responsive.css
├── /js
│   ├── /modules
│   │   ├── auth.js
│   │   ├── visitors.js
│   │   ├── vehicles.js
│   │   └── drivers.js
│   ├── /utils
│   │   ├── validation.js
│   │   └── sanitization.js
│   └── app.js
└── index.html
```

---

## 🟡 PROBLEMAS DE MÉDIA PRIORIDADE

### 8. Acessibilidade
**Problemas:**
- Falta de atributos ARIA
- Sem suporte adequado para navegação por teclado
- Sem suporte para leitores de tela
- Possíveis problemas de contraste de cores
- Sem indicadores de foco visíveis

**Impacto:**
- Dificulta uso por pessoas com deficiência
- Não conformidade com WCAG 2.1
- Possíveis questões legais

---

### 9. Performance
**Problemas:**
- Todos os dados carregados em memória
- Sem paginação para grandes conjuntos de dados
- Manipulação pesada do DOM
- Sem carregamento lazy
- Renderização completa a cada atualização

**Impacto:**
- Lentidão com muitos registros
- Consumo excessivo de memória
- Experiência do usuário degradada

---

### 10. Qualidade do Código
**Problemas:**
- Variáveis globais excessivas
- Sem uso de recursos JavaScript modernos (módulos ES6, classes)
- Convenções de nomenclatura inconsistentes
- Números mágicos no código
- Duplicação de código

**Exemplos de Problemas:**
```javascript
// Variáveis globais
let visitantes = [];
let entradas = [];
let motoristas = [];
// ... 10+ variáveis globais

// Números mágicos
if (diasRestantes < 30) // Por que 30? Deve ser constante

// Duplicação
// Múltiplas funções similares para renderizar listas
```

---

## 📋 RECOMENDAÇÕES PRIORITÁRIAS

### Imediato (Esta Semana)

1. **Implementar Escape HTML**
```javascript
function escapeHtml(unsafe) {
    return unsafe
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");
}

// Uso:
container.innerHTML = `<td>${escapeHtml(visitante.nome)}</td>`;
```

2. **Remover Senhas do localStorage**
```javascript
// Não salvar campo 'senha' no localStorage
const usuariosSemSenha = usuarios.map(u => {
    const {senha, ...resto} = u;
    return resto;
});
localStorage.setItem('portaria_usuarios', JSON.stringify(usuariosSemSenha));
```

3. **Implementar Timeout de Sessão**
```javascript
let sessionTimeout;
function resetSessionTimeout() {
    clearTimeout(sessionTimeout);
    sessionTimeout = setTimeout(() => {
        alert('Sessão expirada por inatividade');
        logout();
    }, 30 * 60 * 1000); // 30 minutos
}
```

### Curto Prazo (Este Mês)

4. **Separar Arquivos**
   - Extrair CSS para arquivo separado
   - Extrair JavaScript para módulos
   - Usar bundler (webpack, vite)

5. **Implementar Validação de Entrada**
   - Criar funções de validação reutilizáveis
   - Adicionar validação no lado do cliente
   - Preparar para validação no servidor

6. **Adicionar Limite de Tentativas de Login**
```javascript
let loginAttempts = {};

function checkLoginAttempts(email) {
    if (!loginAttempts[email]) {
        loginAttempts[email] = {count: 0, lastAttempt: Date.now()};
    }
    
    const attempt = loginAttempts[email];
    if (attempt.count >= 5 && Date.now() - attempt.lastAttempt < 15 * 60 * 1000) {
        return false; // Bloqueado por 15 minutos
    }
    return true;
}
```

### Médio Prazo (Este Trimestre)

7. **Migrar para Framework Moderno**
   - Considerar React, Vue ou Angular
   - Implementar arquitetura baseada em componentes
   - Adicionar gerenciamento de estado (Redux, Vuex)

8. **Implementar Backend**
   - API REST ou GraphQL
   - Autenticação JWT
   - Banco de dados (PostgreSQL, MongoDB)
   - Hash de senha (bcrypt)

9. **Adicionar Testes**
   - Testes unitários (Jest, Vitest)
   - Testes de integração
   - Testes e2e (Cypress, Playwright)

### Longo Prazo (Este Semestre)

10. **Melhorias de Acessibilidade**
    - Implementar ARIA completo
    - Testar com leitores de tela
    - Melhorar navegação por teclado

11. **Otimização de Performance**
    - Implementar virtual scrolling
    - Lazy loading de componentes
    - Cache inteligente

12. **Conformidade e Segurança**
    - Auditoria de segurança profissional
    - Conformidade com LGPD
    - Documentação de segurança

---

## 🎯 PLANO DE AÇÃO SUGERIDO

### Fase 1: Correções Críticas de Segurança (1-2 semanas)
- [ ] Implementar escape HTML em todas as saídas
- [ ] Remover senhas do localStorage
- [ ] Adicionar timeout de sessão
- [ ] Implementar limite de tentativas de login
- [ ] Remover logs sensíveis do console

### Fase 2: Refatoração Inicial (2-4 semanas)
- [ ] Separar HTML, CSS e JavaScript
- [ ] Criar módulos JavaScript
- [ ] Implementar validação de entrada
- [ ] Adicionar testes básicos
- [ ] Melhorar tratamento de erros

### Fase 3: Arquitetura e Backend (1-2 meses)
- [ ] Escolher e configurar framework
- [ ] Desenvolver API backend
- [ ] Implementar autenticação segura
- [ ] Migrar dados para banco de dados
- [ ] Implementar testes automatizados

### Fase 4: Melhorias e Otimização (2-3 meses)
- [ ] Melhorias de acessibilidade
- [ ] Otimizações de performance
- [ ] Documentação completa
- [ ] Treinamento da equipe
- [ ] Monitoramento e logging

---

## 📊 MÉTRICAS DE QUALIDADE

### Atual
- **Linhas de Código:** 4.819 (1 arquivo)
- **Vulnerabilidades Críticas:** 4
- **Vulnerabilidades Altas:** 3
- **Cobertura de Testes:** 0%
- **Conformidade WCAG:** Baixa
- **Score de Segurança:** 2/10

### Meta (6 meses)
- **Linhas de Código:** ~5.000 (30+ arquivos)
- **Vulnerabilidades Críticas:** 0
- **Vulnerabilidades Altas:** 0
- **Cobertura de Testes:** 80%+
- **Conformidade WCAG:** 2.1 AA
- **Score de Segurança:** 8/10

---

## 🔗 RECURSOS E REFERÊNCIAS

### Segurança
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)
- [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)

### Boas Práticas
- [Clean Code JavaScript](https://github.com/ryanmcdermott/clean-code-javascript)
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [MDN Web Docs](https://developer.mozilla.org/)

### Acessibilidade
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [a11y Project Checklist](https://www.a11yproject.com/checklist/)

### LGPD
- [Lei Geral de Proteção de Dados](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
- [Guia LGPD](https://www.gov.br/cidadania/pt-br/acesso-a-informacao/lgpd)

---

## 📝 CONCLUSÃO

O sistema Portaria UNITEC apresenta funcionalidades úteis mas contém várias vulnerabilidades de segurança críticas que devem ser tratadas imediatamente. A refatoração do código e a implementação de boas práticas de desenvolvimento são essenciais para garantir a segurança e a manutenibilidade a longo prazo.

**Prioridade Máxima:** Corrigir vulnerabilidades XSS e problemas de armazenamento de senhas antes de continuar o desenvolvimento de novas funcionalidades.

---

**Revisado por:** Sistema Automatizado de Revisão de Código  
**Próxima Revisão:** Após implementação da Fase 1
