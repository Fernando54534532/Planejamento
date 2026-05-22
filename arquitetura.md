# 1. Estrutura de Pastas do Projeto

A aplicação utiliza o padrão arquitetural em camadas para o ecossistema Express/Node.js, garantindo a separação de responsabilidades (Separation of Concerns).

```txt
meajudaaqui/
├── public/                       
│   ├── css/                      
│   └── images/                   
│
├── src/                          
│   ├── controllers/              
│   │   ├── authController.js     
│   │   ├── ticketController.js   
│   │   └── adminController.js    
│   │
│   ├── services/                 
│   │   ├── authService.js        
│   │   └── ticketService.js      
│   │
│   ├── repositories/             
│   │   ├── userRepository.js     
│   │   └── ticketRepository.js   
│   │
│   ├── middlewares/              
│   │   ├── authMiddleware.js     
│   │   └── adminMiddleware.js    
│   │
│   ├── routes/                   
│   │   ├── authRoutes.js         
│   │   ├── userRoutes.js         
│   │   └── adminRoutes.js        
│   │
│   ├── views/                    
│   └── app.js                    
│
├── package.json                  
└── README.md                     
```

---

# 2. Responsabilidade das Camadas

## Repositories (Camada de Dados)

É a única camada que acessa diretamente as coleções de dados (nesta etapa, os arrays armazenados na memória do servidor). Ela executa operações fundamentais como buscar, criar e atualizar registros, sem conhecer as regras de negócio associadas.

## Services (Camada de Negócio)

Concentra todas as validações, checagens de segurança e fluxos lógicos antes de consolidar uma operação. É onde se valida se um formulário está vazio ou se o status inserido é aceito pelo sistema.

## Controllers (Camada de Controle)

Intermedeia a comunicação entre a interface de usuário (Views) e a lógica interna (Services). Filtra as informações vindas das rotas (`req.body`, `req.params`), aciona as funções de negócio e define qual tela será exibida ou para onde o navegador será redirecionado.

## Middlewares (Camada de Interceptação)

Filtros de barreira que analisam a requisição antes de atingir o Controller. Utilizados para checar o estado de `req.session`, identificando se o usuário está logado e se possui privilégio administrativo.

---

# 3. Organização das Rotas

## authRoutes.js (Públicas)

Gerencia o fluxo inicial de autenticação, cobrindo as URLs:

- `/login`
- `/cadastro`
- `/logout`

## userRoutes.js (Privadas)

Protege o ambiente do usuário comum, regulando o acesso ao histórico de chamados e criação de novos chamados:

- `/chamados`
- `/chamados/novo`

## adminRoutes.js (Privadas de Administrador)

Restringe o acesso às funcionalidades administrativas:

- `/admin/dashboard`
- `/admin/chamados/:id/status`

---

# 4. Entidades do Sistema

## Usuário

```json
{
  "id": "String (UUID ou sequencial)",
  "nome": "String",
  "email": "String",
  "senha": "String (Hash criptografado)",
  "role": "String ('comum' ou 'admin')"
}
```

## Chamado

```json
{
  "id": "String (UUID ou sequencial)",
  "titulo": "String",
  "descricao": "String",
  "categoria": "String ('Hardware', 'Software' ou 'Rede')",
  "status": "String ('Aberto', 'Em Andamento', 'Pendente', 'Concluído', 'Recusado')",
  "dataCriacao": "Date",
  "usuarioId": "String"
}
```

---

# 5. Relacionamento entre Entidades

O sistema utiliza relacionamento do tipo **1 para N (Um para Muitos)**.

- Um usuário pode abrir vários chamados.
- Cada chamado pertence a apenas um usuário.

```txt
Usuário (1) ────────────< Chamados (N)
```
