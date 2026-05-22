💻 1. Diagrama de Estados do Chamado (O Ciclo de Vida)
Este diagrama mostra como o "Status" do chamado muda de acordo com as ações do sistema e do Administrador.

       [ Usuário cria o chamado ]
                   │
                   ▼
             ┌───────────┐
             │  ABERTO   │
             └─────┬─────┘
                   │
         ┌─────────┴─────────┐
         ▼                   ▼
  (Admin assume)      (Admin rejeita)
         │                   │
         ▼                   ▼
┌────────────────┐   ┌───────────────┐
│  EM ANDAMENTO  │   │   RECUSADO    │
└────────┬───────┘   └───────────────┘
         │ (Precisa de info)
         ├───────────────────┐
         ▼                   ▼
┌────────────────┐   ┌───────────────┐
│   CONCLUÍDO    │   │   PENDENTE    │
└────────────────┘   └───────────────┘
