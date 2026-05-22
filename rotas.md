# 🗺️ Definição das Rotas — MeAjudaAqui

> **Disciplina:** Desenvolvimento para Web  
> **Equipe:** Bernardo Augusto Caciamani, William Matheus Barbosa Hernandorena, Fernando Braghini  

---

## 📊 Tabela Geral de Rotas

O sistema está estruturado utilizando o padrão de arquitetura em camadas. Abaixo estão mapeadas todas as URLs da aplicação, divididas por nível de acesso e responsabilidade operacional.

| Método HTTP | Rota (URL) | Tipo de Acesso | Finalidade / Ação do Sistema |
| :---: | :--- | :---: | :--- |
| **Módulo de Autenticação** | | | |
| `GET` | `/` | **Pública** | Renderiza a página de apresentação institucional do sistema. |
| `GET` | `/login` | **Pública** | Exibe o formulário de login para o usuário ou administrador. |
| `POST` | `/login` | **Pública** | Processa os dados de login, valida as credenciais e inicia a sessão (`req.session`). |
| `GET` | `/cadastro` | **Pública** | Exibe o formulário de criação de conta para novos usuários (estudantes/professores). |
| `POST` | `/cadastro` | **Pública** | Processa os dados do formulário e registra o novo usuário no repositório em memória. |
| `GET` | `/logout` | **Privada** | Destrói a sessão ativa do usuário e o redireciona para a página inicial (`/`). |
| **Módulo do Usuário Comum** | | | |
| `GET` | `/chamados` | **Privada (Comum)** | Exibe o dashboard do aluno/professor, listando apenas os chamados criados por ele. |
| `GET` | `/chamados/novo` | **Privada (Comum)** | Exibe o formulário para abertura de um novo chamado de suporte técnico. |
| `POST` | `/chamados/novo` | **Privada (Comum)** | Recebe os dados do chamado, define o status automático como `"Aberto"` e salva no repositório. |
| **Módulo do Administrador** | | | |
| `GET` | `/admin/dashboard` | **Privada (Admin)** | Exibe o dashboard macro, listando **todos** os chamados abertos no sistema para triagem. |
| `POST` | `/admin/chamados/:id/status` | **Privada (Admin)** | Recebe e processa a alteração de status de um chamado específico (Em Andamento, Concluído, Pendente, Recusado). |

---

## 🔒 Detalhamento dos Middlewares de Segurança

Para garantir a integridade das rotas privadas e impedir o acesso não autorizado, a aplicação intercepta as requisições utilizando dois middlewares principais no arquivo `app.js` ou nas rotas específicas:

### 1. `authMiddleware` (Validação de Sessão)
* **Alvo:** Todas as rotas que iniciam com `/chamados` e `/admin`.
* **Regra de Negócio:** Verifica a existência do objeto `req.session.user`. Caso o usuário não esteja autenticado, a requisição é interrompida e ocorre um redirecionamento imediato para `/login` com uma mensagem de aviso.

### 2. `adminMiddleware` (Controle de Perfil)
* **Alvo:** Todas as rotas que iniciam com `/admin`.
* **Regra de Negócio:** Verifica se a propriedade `req.session.user.role` é estritamente igual a `"admin"`. Se um usuário do tipo `"comum"` tentar forçar a URL pelo navegador, o middleware barra a requisição e renderiza uma página personalizada de **Erro 403 (Acesso Proibido)**.

---
