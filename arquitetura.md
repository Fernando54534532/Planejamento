1. ESTRUTURA DE PASTAS DO PROJETO
A aplicação utiliza o padrão arquitetural em camadas para o ecossistema Express/Node.js, garantindo a separação de responsabilidades (Separation of Concerns).

meajudaaqui/
├── public/                       # Arquivos estáticos acessíveis pelo navegador
│   ├── css/                      # Estilização visual das páginas
│   └── images/                   # Logotipos e ícones do sistema
│
├── src/                          # Código-fonte principal da aplicação
│   ├── controllers/              # Gerenciamento de requisições e respostas
│   │   ├── authController.js     # Controle de login e cadastro
│   │   ├── ticketController.js   # Controle de chamados do usuário comum
│   │   └── adminController.js    # Controle do painel de administração
│   │
│   ├── services/                 # Regras de negócio e validações lógicas
│   │   ├── authService.js        # Regras de criação de conta e senha
│   │   └── ticketService.js      # Validação de status e novos chamados
│   │
│   ├── repositories/             # Manipulação e persistência de dados
│   │   ├── userRepository.js     # Persistência de usuários (Array em memória)
│   │   └── ticketRepository.js   # Persistência de chamados (Array em memória)
│   │
│   ├── middlewares/              # Interceptadores de segurança e sessão
│   │   ├── authMiddleware.js     # Bloqueio de usuários não autenticados
│   │   └── adminMiddleware.js    # Bloqueio de acesso a funções de admin
│   │
│   ├── routes/                   # Mapeamento e distribuição das URLs
│   │   ├── authRoutes.js         # Rotas de login, cadastro e logout
│   │   ├── userRoutes.js         # Rotas do painel do aluno/professor
│   │   └── adminRoutes.js        # Rotas de gestão da equipe de TI
│   │
│   ├── views/                    # Telas do sistema (Templates HTML/EJS)
│   └── app.js                    # Arquivo central de configuração do Express
│
├── package.json                  # Gerenciador de dependências e scripts do projeto
└── README.md                     # Manual de instalação e execução do sistema



2. RESPONSABILIDADE DAS CAMADAS
    • Repositories (Camada de Dados): É a única camada que acessa diretamente as coleções de dados (nesta etapa, os arrays armazenados na memória do servidor). Ela executa operações fundamentais como buscar, criar e atualizar registros, sem conhecer as regras de negócio associadas.
    • Services (Camada de Negócio): Concentra todas as validações, checagens de segurança e fluxos lógicos antes de consolidar uma operação. É onde se valida se um formulário está vazio ou se o status inserido é aceito pelo sistema.
    • Controllers (Camada de Controle): Intermedeia a comunicação entre a interface de usuário (Views) e a lógica interna (Services). Filtra as informações vindas das rotas (req.body, req.params), aciona as funções de negócio e dita qual tela será exibida ou para onde o navegador será redirecionamento.
    • Middlewares (Camada de Interceptação): Filtros de barreira que analisam a requisição antes de atingir o Controller. Utilizados para checar o estado de req.session, identificando se o internauta está logado e se possui o privilégio administrativo necessário.
3. ORGANIZAÇÃO DAS ROTAS
    • authRoutes.js (Públicas): Gerencia o fluxo inicial de identificação, cobrindo as URLs /login, /cadastro e o encerramento de sessão em /logout.
    • userRoutes.js (Privadas): Protege o ecossistema do usuário comum, regulando o acesso ao histórico de chamados em /chamados e o envio de novos formulários em /chamados/novo.
    • adminRoutes.js (Privadas de Admin): Restringe o acesso às rotas exclusivas do suporte de TI, operando o painel macro em /admin/dashboard e as alterações de estado em /admin/chamados/:id/status.
4. ENTIDADES DO SISTEMA 

	Usuário
	
	{ 
		"id": "String (UUID ou gerado sequencialmente)",
		"nome": "String",
		"email": "String",
		"senha": "String (Criptografada / Hash)",
		"role": "String ('comum' ou 'admin')"
	} 

	Chamado

	{
 		 "id": "String (UUID ou gerado sequencialmente)",
		  "titulo": "String",
  		"descricao": "String",
  		"categoria": "String ('Hardware', 'Software' ou 'Rede')",
  		"status": "String ('Aberto', 'Em Andamento', 'Pendente', 'Concluído', 		'Recusado')",
		 "dataCriacao": "Date",
  		"usuarioId": "String (Vínculo com o ID do Usuário Criador)"
}	

5. RELACIONAMENTO
O mapeamento de dados do sistema adota o modelo 1 para N (Um para Muitos):
    • Um Usuário (aluno, líder ou professor) tem a permissão de registrar e acompanhar Muitos Chamados dentro da plataforma.
    • Um Chamado estará estritamente vinculado a Um único Usuário solicitante, utilizando o campo usuarioId como chave de rastreabilidade.
$$\text{Usuário (1)} \longrightarrow \text{Abre / Acompanha} \longrightarrow \text{Chamados (N)}$$
