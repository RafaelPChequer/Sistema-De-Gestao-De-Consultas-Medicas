# Documentação do Projeto: Sistema de Gestão de Consultas Médicas - Vida+ Saúde

## Integrantes
- Davi Aguilar
- Pedro Moreira
- Rafael Chequer

## 🏗️ Arquitetura e Governança

### Estilo Arquitetural
O sistema adota a **Arquitetura em Camadas** com uma abordagem **monolítica**, utilizando **API RESTful** para comunicação entre front-end e back-end. Essa escolha é adequada para o sistema de gestão de consultas médicas da Vida+ Saúde, que possui funcionalidades bem definidas e não demanda integrações externas complexas. A arquitetura monolítica simplifica desenvolvimento, testes e implantação, enquanto o padrão em camadas garante modularidade e facilidade de manutenção.

### Padrão em Camadas
O sistema é organizado em três camadas:
1. **Apresentação**: Interface do usuário, desenvolvida com React.js e estilizada com Tailwind CSS.
2. **Lógica de Negócios**: Back-end implementado com Java Spring Boot, responsável por regras de negócio e processamento de requisições.
3. **Dados**: Banco de dados relacional PostgreSQL para persistência de dados estruturados.

Essa estrutura promove:
- **Separação de responsabilidades**: Cada camada tem funções específicas, reduzindo dependências.
- **Manutenibilidade**: Alterações em uma camada têm impacto mínimo nas demais.
- **Simplicidade**: Ideal para um projeto de porte médio com requisitos claros.

### Governança do Projeto
A governança utiliza recursos do GitHub:
- **Commits**: Alterações documentadas com mensagens descritivas.
- **Pull Requests**: Revisão de código antes da integração.
- **Issues**: Registro de tarefas, bugs e melhorias.
- **Branches**: Desenvolvimento isolado de funcionalidades em branches separadas.

### Gestão de Tarefas
As tarefas são gerenciadas na aba **Projects** do GitHub, com um quadro **Kanban** dividido em:
- **A Fazer**: Tarefas pendentes.
- **Em Progresso**: Tarefas em andamento.
- **Concluído**: Tarefas finalizadas.

Cada tarefa é uma **issue** com responsável, prazo e descrição, garantindo:
- Planejamento claro.
- Priorização eficiente.
- Colaboração entre a equipe.

### Stack e Comunicação
- **Front-end**: React.js (v18.x) com Tailwind CSS para estilização responsiva.
- **Back-end**: Java com Spring Boot (v3.x) para APIs robustas.
- **Comunicação**: API RESTful com JSON, utilizando métodos HTTP (GET, POST, PUT, DELETE).
- **Autenticação**: JWT para proteção de rotas e controle de acesso.

### Segurança
- **Autenticação**: JWT para autenticação de usuários (Paciente, Médico, Administrador).
- **Criptografia de Senhas**: Senhas armazenadas com bcrypt.
- **HTTPS**: Segurança na transmissão de dados.
- **Validação**: Esquemas OpenAPI para validação de requisições.

### Características do Projeto
- **Baixo Acoplamento**: Camadas bem separadas, embora o monolito possa aumentar o acoplamento em projetos maiores.
- **Escalabilidade**: Suporta crescimento moderado; refatoração para microserviços pode ser necessária em alta demanda.
- **Manutenibilidade**: Código modular e documentação clara.
- **Simplicidade**: Stack consolidada (React, Spring Boot, PostgreSQL) com ampla documentação.

---

## 🎨 Style Guide

### Light Mode
#### Identidade Visual
| Nome            | Código HEX | Uso Principal                     |
|-----------------|------------|-----------------------------------|
| Primária        | #1E90FF    | Botões, links, bordas             |
| Secundária      | #32CD32    | Ações relacionadas à saúde        |
| Neutro Claro    | #F5F5F5    | Fundos de cards e telas           |
| Texto Principal | #000000    | Textos principais                 |
| Erro            | #FF0000    | Alertas e mensagens de erro       |
| Branco          | #FFFFFF    | Fundos principais                 |

### Dark Mode
#### Identidade Visual
| Nome            | Código HEX | Uso Principal                     |
|-----------------|------------|-----------------------------------|
| Fundo Principal | #121212    | Fundo geral da tela               |
| Fundo de Cards  | #1C1C1C    | Fundos de cards e inputs          |
| Texto Principal | #FFFFFF    | Textos principais                 |
| Texto Secundário| #B0B0B0    | Textos auxiliares, placeholders   |
| Borda de Inputs | #333333    | Bordas de inputs e cards          |
| Primária        | #1E90FF    | Botões, links, bordas             |
| Erro            | #FF0000    | Alertas e mensagens de erro       |

### Tipografia
| Tipo      | Fonte Principal | Peso       | Uso                          |
|-----------|-----------------|------------|------------------------------|
| Títulos   | Roboto          | Bold (700) | Títulos e cabeçalhos         |
| Corpo     | Roboto          | Medium (500)| Textos de botões e links     |
| Auxiliar  | Roboto          | Regular (400)| Textos de inputs e descrições|

### Componentes UI
#### Botões
| Variante     | Cor       | Borda       | Texto   | Uso                          |
|--------------|-----------|-------------|---------|------------------------------|
| Primário     | #1E90FF   | 4px         | Branco  | Ações principais (agendar, login) |
| Secundário   | #FF0000   | 4px         | Branco  | Ações de cancelamento        |
| Desabilitado | #D1D5DB   | Nenhum      | #9CA3AF | Estado inativo               |

#### Inputs
- **Altura**: 48px
- **Bordas Arredondadas**: 8px
- **Placeholder**:
  - Light Mode: #1C1B1F
  - Dark Mode: #B0B0B0
- **Tipos**: Text, Password, Email, Date, Select
- **Fundo**:
  - Light Mode: #FFFFFF
  - Dark Mode: #1C1C1C
- **Borda**:
  - Light Mode: #C3C3C3
  - Dark Mode: #333333

#### Cards
- **Fundo**:
  - Light Mode: #FFFFFF
  - Dark Mode: #1C1C1C
- **Borda**: 1px sólida
  - Light Mode: #1E90FF
  - Dark Mode: #333333
- **Bordas Arredondadas**: 10px
- **Padding Interno**: 16px

#### Ícones
- **Tamanho Padrão**: 24px
- **Cor**:
  - Light Mode: #000000 ou #1E90FF
  - Dark Mode: #FFFFFF ou #1E90FF

### Como Alternar entre Light e Dark Mode
A troca de temas ajusta:
- Fundo da tela e cards.
- Cor de textos, ícones e bordas de inputs.
- Cores de botões mantêm consistência, com ajustes para hover/desabilitado.

### Decisões de Acessibilidade e Boas Práticas
- **Contraste**: Atende WCAG 2.1 (mínimo 4.5:1 para textos).
- **Área Clicável**: Botões e inputs com tamanho mínimo de 44px.
- **Tipografia**: Roboto garante legibilidade e suporte a múltiplos pesos.
- **Navegação por Teclado**: Suporte completo para acessibilidade.
- **ARIA Labels**: Componentes interativos com atributos ARIA.
- **Consistência Visual**: Cores primária (#1E90FF) e secundária (#32CD32) mantidas para identidade da marca.

---

## 🗄️ Banco de Dados

### SGBD Utilizado
- **PostgreSQL**: Banco relacional open-source, escolhido por sua robustez, suporte a SQL padrão e transações ACID.
- **MongoDB Atlas** (opcional): Para logs ou dados não estruturados, se necessário.

### Diagrama
Tabelas principais:
1. **TB_USUARIOS**
   - `id_usuario` (PK, bigint): Identificador único.
   - `nm_nome` (varchar): Nome completo.
   - `em_email` (varchar, unique): E-mail.
   - `ds_senha` (varchar): Senha criptografada (bcrypt).
   - `nr_cpf` (varchar, unique): CPF.
   - `nr_crm` (varchar, nullable): CRM (médicos).
   - `nm_especialidade` (varchar, nullable): Especialidade (médicos).
   - `ds_tipo` (enum): PACIENTE, MEDICO, ADMIN.

2. **TB_CONSULTAS**
   - `id_consulta` (PK, bigint): Identificador único.
   - `fk_paciente` (FK, bigint): Referência a TB_USUARIOS.
   - `fk_medico` (FK, bigint): Referência a TB_USUARIOS.
   - `dt_consulta` (date): Data da consulta.
   - `hr_consulta` (time): Hora da consulta.
   - `ds_status` (enum): AGENDADA, CANCELADA, CONCLUIDA.

3. **TB_ESPECIALIDADES**
   - `id_especialidade` (PK, bigint): Identificador único.
   - `nm_especialidade` (varchar): Nome da especialidade.

### Diretrizes do Banco de Dados
#### 1. Convenções de Nomenclatura
- Nomes com **underscore** (ex.: `id_usuario`).
- Prefixos:
  - `id_`: Identificador único.
  - `fk_`: Chave estrangeira.
  - `nm_`: Nome/descrição.
  - `em_`: E-mail.
  - `nr_`: Número.
  - `dt_`: Data.
  - `hr_`: Hora.
  - `ds_`: Status/descrição.

#### 2. Relacionamentos
- **Chaves estrangeiras** para integridade referencial.
- `TB_USUARIOS` generaliza `PACIENTE`, `MEDICO` e `ADMIN`, com `ds_tipo` definindo o papel.

#### 3. Regras de Negócio
- **Consultas**:
  - Bloquear horários inválidos ou ocupados.
  - Status: AGENDADA, CONCLUIDA, CANCELADA.
- **Usuários**:
  - E-mails únicos e válidos.
  - Senhas com 9+ caracteres, letra maiúscula, caractere especial e número.
  - CRM válido e único para médicos.

#### 4. Segurança
- **Controle de Acesso**: Perfis com permissões específicas.
- **Criptografia**: Senhas com bcrypt; dados sensíveis protegidos.
- **Conexões**: SSL/TLS para segurança.

### Limitações e Melhorias
| Item                          | Situação no PostgreSQL         | Melhorias Implementáveis                     |
|-------------------------------|--------------------------------|---------------------------------------------|
| Criptografia em repouso       | Disponível nativamente         | Configurar TDE.                             |
| Auditoria detalhada           | Suporte via pgaudit            | Habilitar pgaudit para logs.                |
| Controle de acesso por linha  | Suporte nativo (RLS)           | Implementar RLS para restrições por usuário.|

---

## 🎯 Motivação
O Sistema de Gestão de Consultas Médicas da Vida+ Saúde substitui processos manuais por uma solução digital eficiente, promovendo:
- **Escalabilidade**: Suporte a novas funcionalidades (ex.: notificações).
- CLEAR **Usabilidade**: Interfaces acessíveis para todos os usuários.
- **Eficiência**: Automação de agendamentos e relatórios.
- **Segurança**: Proteção de dados com autenticação e criptografia.

A arquitetura em camadas e o Design System garantem uma experiência confiável, alinhada aos objetivos de modernização da Vida+ Saúde.

