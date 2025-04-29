# Documentação do Projeto: Sistema de Gestão de Consultas Médicas - Vida+ Saúde

## Estilo Arquitetural
O estilo arquitetural escolhido para o Sistema de Gestão de Consultas Médicas é a **Arquitetura em 3 Camadas** (Apresentação, Negócio e Dados), complementada por princípios de **Micro Frontends** para o front-end e **API RESTful** para comunicação entre camadas. Essa escolha promove modularidade, escalabilidade e manutenibilidade, alinhando-se às necessidades da clínica Vida+ Saúde.

### Padrões Arquiteturais
Os padrões arquiteturais adotados são:
- **MVC (Model-View-Controller)**: Separação clara entre interface do usuário (View), lógica de negócio (Controller) e dados (Model), facilitando manutenção e testes.
- **API Gateway**: Centraliza requisições HTTP, roteando-as para os serviços apropriados e aplicando políticas de segurança.
- **Component-Based Architecture**: No front-end, utiliza componentes reutilizáveis para garantir consistência visual e funcional.
- **Token JWT (JSON Web Tokens)**: Autenticação segura para proteger endpoints e gerenciar sessões de usuários.

---

## 🔐 BackEnd
### Requisitos de Segurança
- **OAuth2**: Autenticação e autorização segura para pacientes, médicos e administradores.
- **HTTPS**: Garante a segurança na transmissão de dados sensíveis.
- **JWT**: Tokens para autenticação de sessões e validação de permissões por tipo de usuário (Paciente, Médico, Administrador).
- **Criptografia de Dados Sensíveis**: Senhas armazenadas com hash (ex.: bcrypt) e dados pessoais (como CPF) protegidos no banco.

### Protocolo de Comunicação
- **API REST**: Comunicação entre front-end e back-end via HTTP, utilizando métodos padrão (GET, POST, PUT, DELETE) e respostas JSON.
- **Formato de Dados**: JSON para troca de dados, com validação de esquemas via OpenAPI.

### Tecnologias Utilizadas
- **Java com Spring Boot (versão 3.x)**: Framework principal para desenvolvimento do back-end, com suporte a API REST, injeção de dependências e validação.
- **Spring Security**: Gerenciamento de autenticação e autorização.
- **JPA/Hibernate**: Comunicação com o banco de dados relacional para persistência de dados.
- **Lombok**: Redução de boilerplate no código Java.
- **Maven**: Gerenciamento de dependências e build.

### Endpoints Principais (Baseado no OpenAPI fornecido)
Os endpoints seguem a especificação OpenAPI 3.0.0 fornecida, com adições para suportar o sistema completo:

1. **/cadastro/paciente** (POST)
   - **Descrição**: Cadastra um novo paciente.
   - **Parâmetros**:
     - `nome` (string, obrigatório): Nome completo do paciente.
     - `email` (string, obrigatório): E-mail único.
     - `senha` (string, obrigatório): Senha com validação de força.
     - `cpf` (string, obrigatório): CPF único.
   - **Respostas**:
     - `201`: Paciente cadastrado com sucesso.
     - `400`: Erro de validação (ex.: formato de e-mail inválido).
     - `409`: Conflito (e-mail ou CPF já cadastrado).

2. **/cadastro/medico** (POST)
   - **Descrição**: Cadastra um novo médico.
   - **Parâmetros**:
     - `nome` (string, obrigatório): Nome completo do médico.
     - `email` (string, obrigatório): E-mail único.
     - `senha` (string, obrigatório): Senha com validação.
     - `cpf` (string, obrigatório): CPF único.
     - `crm` (string, obrigatório): CRM do médico.
     - `especialidade` (string, obrigatório): Especialidade médica.
   - **Respostas**:
     - `201`: Médico cadastrado com sucesso.
     - `400`: Erro de validação.
     - `409`: Conflito (e-mail, CPF ou CRM já cadastrado).

3. **/login** (POST)
   - **Descrição**: Autentica um usuário (Paciente, Médico ou Administrador).
   - **Parâmetros**:
     - `identificador` (string, obrigatório): E-mail ou CPF.
     - `senha` (string, obrigatório): Senha do usuário.
   - **Respostas**:
     - `200`: Login bem-sucedido, retorna token JWT.
     - `401`: Credenciais inválidas.
     - `403`: Acesso negado para o tipo de usuário.

4. **/consultas/agendamento** (POST)
   - **Descrição**: Agenda uma consulta para um paciente com um médico.
   - **Parâmetros**:
     - `pacienteId` (integer, obrigatório): ID do paciente.
     - `medicoId` (integer, obrigatório): ID do médico.
     - `data` (string, obrigatório): Data da consulta (formato ISO 8601).
     - `hora` (string, obrigatório): Hora da consulta.
   - **Respostas**:
     - `201`: Consulta agendada com sucesso.
     - `400`: Horário indisponível ou dados inválidos.
     - `403`: Permissão insuficiente.

5. **/consultas/cancelamento/{id}** (DELETE)
   - **Descrição**: Cancela uma consulta com aviso prévio ao médico.
   - **Parâmetros**:
     - `id` (integer, path): ID da consulta.
   - **Respostas**:
     - `200`: Consulta cancelada com sucesso.
     - `404`: Consulta não encontrada.
     - `403`: Permissão insuficiente.

6. **/relatorios/consultas** (GET)
   - **Descrição**: Gera relatórios de consultas por período.
   - **Parâmetros**:
     - `dataInicio` (string, query): Data inicial (ISO 8601).
     - `dataFim` (string, query): Data final (ISO 8601).
   - **Respostas**:
     - `200`: Relatório gerado com sucesso.
     - `400`: Parâmetros inválidos.

### Diagrama Arquitetural
```
[Usuário] --> [API Gateway] --> [Auth Service] --> [JWT Validation]
                          --> [Consulta Service] --> [JPA/PostgreSQL]
                          --> [Relatório Service] --> [JPA/PostgreSQL]
                          --> [Usuário Service] --> [JPA/PostgreSQL]
```

---

## 🎨 FrontEnd
### Padrões de Acessibilidade
- **WCAG 2.1**: Conformidade com diretrizes de acessibilidade (nível AA).
- **Alt Text**: Todas as imagens com descrições alternativas.
- **Navegação por Teclado**: Suporte completo para navegação sem mouse.
- **Contraste**: Relação de contraste mínima de 4.5:1 para textos.
- **ARIA Labels**: Uso de atributos ARIA para componentes interativos.

### Jornada do Usuário
1. **Paciente**:
   - Faz login com e-mail ou CPF.
   - Navega para a tela de agendamento, seleciona médico, data e hora.
   - Confirma ou cancela consultas.
   - Visualiza histórico de consultas.
2. **Médico**:
   - Faz login e visualiza agendamentos do dia.
   - Acessa detalhes de consultas (paciente, horário).
3. **Administrador**:
   - Faz login e gerencia consultas (visualização, edição, cancelamento).
   - Gera relatórios de consultas por período.

### Design
- **Cores**:
  - Primária: Azul (#1E90FF) – Transmite confiança e profissionalismo.
  - Secundária: Verde (#32CD32) – Representa saúde e vitalidade.
  - Neutra: Branco (#FFFFFF), Cinza Claro (#F5F5F5).
- **Tipografia**: Roboto (Google Fonts) – Moderna e legível.
- **Ícones**: Material Icons – Consistência e ampla biblioteca.
- **Framework CSS**: Tailwind CSS – Estilização rápida e responsiva.

### Tecnologias Utilizadas
- **React (versão 18.x)**: Framework principal para construção do front-end.
- **React Router**: Gerenciamento de rotas para navegação.
- **Axios**: Comunicação com a API REST.
- **Vite**: Ferramenta de build para desenvolvimento rápido.
- **ESLint + Prettier**: Padronização de código.

### Estrutura de Componentes
```
/src
├── components/
│   ├── atoms/
│   │   ├── Button.jsx
│   │   ├── Input.jsx
│   │   └── Icon.jsx
│   ├── molecules/
│   │   ├── FormField.jsx
│   │   ├── CardConsulta.jsx
│   │   └── ModalConfirmacao.jsx
│   ├── organisms/
│   │   ├── Navbar.jsx
│   │   ├── AgendamentoForm.jsx
│   │   └── TabelaConsultas.jsx
├── pages/
│   ├── Login.jsx
│   ├── Agendamento.jsx
│   ├── MedicoDashboard.jsx
│   └── AdminDashboard.jsx
├── hooks/
│   ├── useAuth.js
│   ├── useConsultas.js
├── services/
│   ├── api.js
├── styles/
│   ├── tailwind.css
├── App.jsx
├── main.jsx
```

---

## 🗄️ Dados
### Banco de Dados
- **SGBD Utilizado**: PostgreSQL – Banco relacional para dados estruturados (usuários, consultas, especialidades).
- **MongoDB Atlas** (opcional): Para logs e histórico de eventos, caso haja necessidade de dados não estruturados.

### Modelo de Dados
#### Tabelas Principais
1. **TB_USUARIOS**
   - `CD_USUARIO` (PK, bigint): Identificador único.
   - `NM_NOME` (varchar): Nome completo.
   - `DS_EMAIL` (varchar, unique): E-mail do usuário.
   - `DS_SENHA` (varchar): Senha criptografada.
   - `DS_CPF` (varchar, unique): CPF do usuário.
   - `DS_CRM` (varchar, nullable): CRM (apenas para médicos).
   - `DS_ESPECIALIDADE` (varchar, nullable): Especialidade (apenas para médicos).
   - `DS_TIPO` (enum): Tipo de usuário (PACIENTE, MEDICO, ADMIN).

2. **TB_CONSULTAS**
   - `CD_CONSULTA` (PK, bigint): Identificador único.
   - `CD_PACIENTE` (FK, bigint): Referência a TB_USUARIOS.
   - `CD_MEDICO` (FK, bigint): Referência a TB_USUARIOS.
   - `DT_CONSULTA` (date): Data da consulta.
   - `HR_CONSULTA` (time): Hora da consulta.
   - `DS_STATUS` (enum): Status (AGENDADA, CANCELADA, CONCLUIDA).

3. **TB_ESPECIALIDADES**
   - `CD_ESPECIALIDADE` (PK, bigint): Identificador único.
   - `NM_ESPECIALIDADE` (varchar): Nome da especialidade (ex.: Cardiologia).

#### Diagrama de Entidade-Relacionamento (DER)
```
TB_USUARIOS       TB_CONSULTAS       TB_ESPECIALIDADES
| CD_USUARIO |<---| CD_PACIENTE |     | CD_ESPECIALIDADE |
| NM_NOME    |    | CD_MEDICO  |---->| NM_ESPECIALIDADE |
| DS_EMAIL   |    | DT_CONSULTA|
| DS_SENHA   |    | HR_CONSULTA|
| DS_CPF     |    | DS_STATUS  |
| DS_CRM     |
| DS_TIPO    |
```

### Ferramentas de Modelagem
- **dbdiagram.io**: Rascunho inicial do modelo de dados.
- **MySQL Workbench**: Refinamento e validação do modelo.
- **PgAdmin**: Gerenciamento do banco PostgreSQL.

---

## 🎨 Design System
### Visão Geral
O Design System do Sistema de Gestão de Consultas Médicas é uma biblioteca compartilhada que garante consistência visual, funcional e técnica em todas as interfaces. Ele é estruturado com base no conceito **Atomic Design**, dividindo componentes em camadas (Base, Átomos, Moléculas, Organismos, Templates).

### Estrutura do Design System
```
/design-system
├── tokens/
│   ├── colors.json
│   ├── spacing.json
│   ├── typography.json
├── components/
│   ├── atoms/
│   │   ├── Button/
│   │   │   ├── Button.jsx
│   │   │   ├── Button.stories.js
│   │   │   ├── Button.test.js
│   │   │   └── Button.module.css
│   │   ├── Input/
│   │   ├── Icon/
│   ├── molecules/
│   │   ├── FormField/
│   │   ├── CardConsulta/
│   │   ├── ModalConfirmacao/
│   ├── organisms/
│   │   ├── Navbar/
│   │   ├── AgendamentoForm/
│   │   ├── TabelaConsultas/
├── utils/
│   ├── accessibility.js
│   ├── formatters.js
├── themes/
│   ├── light.json
│   ├── dark.json
├── docs/
│   ├── introduction.md
│   ├── components.md
├── index.js
```

### Tokens de Design
- **Cores**:
  - `--primary`: #1E90FF
  - `--secondary`: #32CD32
  - `--background`: #FFFFFF
  - `--neutral`: #F5F5F5
- **Espaçamento**:
  - `--spacing-xs`: 4px
  - `--spacing-sm`: 8px
  - `--spacing-md`: 16px
  - `--spacing-lg`: 24px
- **Tipografia**:
  - `--font-family`: 'Roboto', sans-serif
  - `--font-size-base`: 16px
  - `--font-weight-regular`: 400
  - `--font-weight-bold`: 700

### Componentes Reutilizáveis
1. **Átomos**:
   - **Button**: Botões primários, secundários e de ação (ex.: "Agendar", "Cancelar").
   - **Input**: Campos de texto, e-mail, senha, etc., com validação visual.
   - **Icon**: Ícones do Material Icons.
2. **Moléculas**:
   - **FormField**: Combinação de Input + Label + Mensagem de erro.
   - **CardConsulta**: Exibe informações de uma consulta (médico, data, hora).
   - **ModalConfirmacao**: Modal para confirmação de ações (ex.: cancelamento).
3. **Organismos**:
   - **Navbar**: Barra de navegação com links contextuais por tipo de usuário.
   - **AgendamentoForm**: Formulário completo para agendamento de consultas.
   - **TabelaConsultas**: Tabela com filtros para visualização de consultas.

### Diretrizes
- **Acessibilidade**: Conformidade com WCAG 2.1 (ex.: contraste, ARIA).
- **Responsividade**: Componentes adaptáveis a telas de 320px a 1920px.
- **Documentação**: Cada componente possui descrição, exemplos de uso e testes no Storybook.

### Ferramentas Utilizadas
- **Storybook**: Documentação viva dos componentes, com suporte a testes visuais.
- **Figma**: Prototipagem de telas e definição de tokens visuais.
- **Notion**: Documentação de decisões de arquitetura e boas práticas.
- **Vercel**: Publicação do Design System para acesso interno.

---

## 🛠️ Implementação do Desafio Prático
### Passo 1: Acesse o diretório System Design Primer
- O **System Design Primer** (disponível no GitHub) foi consultado para embasar a arquitetura escalável do sistema, com foco em:
  - **Escalabilidade**: Uso de API Gateway para balanceamento de carga.
  - **Segurança**: Autenticação via JWT e OAuth2.
  - **Modularidade**: Separação clara entre camadas e serviços.

### Passo 2: Estudo do Material
- O material foi analisado para garantir que a arquitetura proposta siga boas práticas, como:
  - **Separação de Responsabilidades**: Cada camada (Apresentação, Negócio, Dados) tem funções bem definidas.
  - **Reutilização**: Design System como biblioteca compartilhada.
  - **Manutenibilidade**: Componentes modulares e documentação clara.

### Passo 3: Estrutura do Design System
A estrutura proposta segue o modelo descrito acima, com ênfase em:
- **Escalabilidade**: Componentes distribuídos via npm para reutilização em outros projetos da clínica.
- **Consistência**: Tokens de design centralizados para evitar divergências visuais.
- **Estilo Arquitetural**: Alinhamento com Micro Frontends, permitindo que diferentes módulos do sistema (ex.: paciente, médico) sejam desenvolvidos independentemente.

### Passo 4: Aplicação ao Estudo de Caso
O Design System foi projetado para atender às telas especificadas:
- **Tela de Login**: Usa componentes `FormField`, `Button` e `ModalConfirmacao` para mensagens de erro.
- **Tela de Agendamento**: Integra `AgendamentoForm` e `CardConsulta` para seleção de médicos e horários.
- **Tela do Médico**: Usa `TabelaConsultas` para exibir agendamentos do dia.
- **Tela Administrativa**: Combina `TabelaConsultas` com filtros e botões de ação.

### Passo 5: Ferramentas para Documentação
- **Storybook**: Documentação técnica dos componentes, com exemplos interativos.
- **Figma**: Prototipagem das telas e definição de tokens visuais.
- **Notion**: Registro de decisões de design, arquitetura e boas práticas.
- **Vercel**: Hospedagem da documentação para acesso interno pela equipe.

---

## 🎯 Motivação
O Sistema de Gestão de Consultas Médicas da Vida+ Saúde visa modernizar a operação da clínica, substituindo processos manuais por uma solução digital eficiente. A arquitetura em 3 camadas, aliada ao Design System, garante:
- **Escalabilidade**: Capacidade de adicionar novas funcionalidades (ex.: telemedicina).
- **Usabilidade**: Interfaces consistentes e acessíveis para todos os usuários.
- **Eficiência**: Automação de agendamentos, cancelamentos e relatórios, reduzindo carga administrativa.
- **Segurança**: Proteção de dados sensíveis com autenticação robusta e criptografia.

Essa abordagem proporciona uma experiência fluida para pacientes, médicos e administradores, alinhando-se aos objetivos de modernização e qualidade da Vida+ Saúde.
