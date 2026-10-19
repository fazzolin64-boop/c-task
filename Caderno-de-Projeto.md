## 1. Visão Geral

Um acompanhador de tarefas e estudos focado em velocidade e organização, operado diretamente pelo Terminal (CMD/Bash). Em vez de uma simples lista de afazeres, o programa divide objetivos maiores (Tarefas) em pequenas etapas (Checkpoints), calculando o progresso real de conclusão.

- **Plataforma Inicial:** Linha de comando (Terminal) em C puro, com foco em rapidez de digitação e visual limpo usando cores e barras de progresso.
    
- **Visão de Futuro:** Possibilidade de migrar ou criar uma interface gráfica ou recriar em outra linguagem aproveitando a mesma base de dados.
    

## 2. Como o Programa Funciona (Fluxo Principal)

### A. Estrutura das Tarefas

- **Tarefa (Cabeçalho):** Possui um nome curto e direto (ex: _"Apresentação de Banco de Dados"_) para funcionar como um título visual. O sistema permite ter várias tarefas ativas ao mesmo tempo.
    
- **Checkpoints (Sub-tarefas):** São os passos que compõem a tarefa (ex: _"Índices GiST"_, _"Índices GIN"_, _"Triggers"_). São eles que ditam a porcentagem da barra de progresso.
    

### B. Duas Formas de Criar Tarefas

1. **Modo Rápido (Linha Única):** Para não perder tempo nem produtividade respondendo perguntas passo a passo, você pode criar a tarefa e todos os checkpoints de uma vez só em um único comando separado por vírgulas:
    
    - _Exemplo:_ `task new "Apresentação BD" -c "Índices GiST, Índices GIN, Triggers"`
        
2. **Modo Guiado (Passo a Passo):** Caso prefira, o programa pergunta o nome da tarefa e depois pede para digitar os checkpoints um por um até finalizar.
    

### C. Gerenciamento no Dia a Dia

- **Painel de Progresso:** Ao listar suas tarefas, o programa exibe uma barra de progresso visual e mostra claramente quais checkpoints estão concluídos `[x]` e quais ainda estão pendentes `[ ]`.
    
- **Edição Flexível:** Mesmo depois que uma tarefa já foi criada, você pode a qualquer momento:
    
    - Marcar ou desmarcar um checkpoint como concluído.
        
    - Adicionar novos checkpoints que surgiram no meio do caminho.
        
    - Remover checkpoints que não são mais necessários.
        
- **Histórico de Conclusão:** Tarefas que chegam a 100% saem da tela principal de pendências e vão para um histórico, mantendo a tela limpa sem perder o registro do que você já produziu.
    

## 3. Funcionalidades Extras (Os Diferenciais do Projeto)

- **Pesos nos Checkpoints (Progresso Inteligente):**
    
    - Nem toda etapa dá o mesmo trabalho. Você pode dar um peso (1, 2 ou 3) para cada checkpoint. Concluir uma etapa difícil (peso 3) faz a barra de progresso avançar muito mais do que uma etapa rápida (peso 1).
        
- **Cronômetro de Foco (Tempo de Estudo):**
    
    - Ao iniciar o estudo de uma tarefa (`task start`), o programa marca o horário atual. Quando você conclui o checkpoint, ele calcula e salva automaticamente quantos minutos ou horas você levou naquela etapa, somando o tempo total investido na tarefa.
        
- **Prazos (Deadlines) com Alertas de Cor:**
    
    - Você pode definir uma data de entrega para a tarefa. O programa lê a data atual do computador e muda a cor do título no terminal:
        
        - **Verde/Azul:** Prazo tranquilo (mais de 3 dias).
            
        - **Amarelo/Vermelho:** Urgente (entrega para amanhã ou hoje).
            
- **Exportação para Markdown (`.md`):**
    
    - Um comando (`task export`) que gera um arquivo `.md` bonitinho com suas tarefas e barras de progresso para abrir no VS Code ou colocar no GitHub.
        

## 4. Plano de Evolução (Fases de Desenvolvimento)

Para aprender tudo sem atropelar etapas, o projeto será construído em versões:

- **Versão 1.0 (Núcleo em C Puro):**
    
    - Criação de tarefas, gerenciamento de checkpoints, barra de progresso simples e salvamento automático em **arquivo local** (texto/binário).
        
- **Versão 1.5 (Produtividade):**
    
    - Adição do comando de linha única (Modo Rápido), cores no terminal, prazos, pesos nos checkpoints e cronômetro de tempo.
        
- **Versão 2.0 (Upgrade para Banco de Dados SQL):**
    
    - Substituição dos arquivos simples por um banco de dados relacional (**SQLite** ou **PostgreSQL**), guardando as tarefas, checkpoints e histórico de tempo em tabelas reais com comandos SQL (`INSERT`, `UPDATE`, `SELECT`).


## 5. Modelo de Interação e Comandos (Estilo Git / Stateless CLI)

Em vez de prender o usuário dentro de um "menu infinito" numérico (`Digite 1 para X, 2 para Y`), o software funcionará no formato **Stateless CLI** (Linha de Comando Direta), inspirado em ferramentas como o próprio **Git**.

* **Como funciona na prática:** Você digita o comando completo no terminal; o programa abre em milissegundos, lê os dados salvos, executa a ação solicitada, imprime o retorno visual na tela e **encerra imediatamente**, devolvendo o terminal livre para o seu uso.

### Mapa de Comandos Planejados

* **Painel Principal (Apenas em andamento):**
  * `task` ou `task status`
  * *O que faz:* Mostra na tela apenas as tarefas ativas no momento, com suas respectivas barras de progresso e a lista numerada de checkpoints (`[ ]` ou `[x]`). Mantém o foco apenas no que falta fazer.

* **Criação Rápida (Linha Única):**
  * `task new "<Nome da Tarefa>" -c "<Check 1>, <Check 2>, <Check 3>"`
  * *Exemplo:* `task new "Apresentação BD" -c "Índices GiST, Índices GIN, Triggers"`
  * *O que faz:* Cria a tarefa cabeçalho e já fatia o texto separado por vírgulas, cadastrando todos os checkpoints de uma vez só. Retorna apenas uma confirmação rápida: `✔ Tarefa [#1] registrada!`.

* **Concluir um Checkpoint (3 Formas Ágeis):**
  1. **Sequencial Automático (`task next <id_tarefa>`):** Marca automaticamente o primeiro checkpoint pendente `[ ]` daquela tarefa na ordem em que foi criado, e já avisa qual é o próximo da fila. Ideal para seguir o roteiro de estudo sem precisar lembrar o número da etapa.
  2. **Por Busca de Texto (`task done "< trecho do nome >"`):** Procura qualquer checkpoint ativo que contenha a palavra digitada (ex: `task done triggers`) e o marca como concluído.
  3. **Direto por ID (`task done <id_tarefa> <id_check>`):** Marca diretamente pelo número (ex: `task done 1 2`) caso você já tenha acabado de olhar o painel do `task status`.

* **Editar Tarefas Existentes (Adicionar ou Remover Etapas):**
  * `task add <id_tarefa> "<Novo Checkpoint>"` *(Adiciona uma nova etapa ao final da tarefa especificada).*
  * `task rm <id_tarefa> <id_checkpoint>` *(Remove um checkpoint específico de uma tarefa).*
  * `task del <id_tarefa>` *(Apaga a tarefa inteira).*

* **Consultar Histórico (Tarefas Concluídas):**
  * `task history` (ou `task list --done`)
  * *O que faz:* Exibe a lista limpa de todas as tarefas que já atingiram 100% no passado, separadas do painel principal para não poluir a visão do dia a dia.

### Exemplo Visual de Saída no Terminal

```text
> task

 📌 TAREFAS ATIVAS (2)
 ──────────────────────────────────────────────────────────
 [#1] Apresentação BD                 [██████░░░░] 66% (2/3)
      [x] 1. Índices GiST
      [x] 2. Índices GIN
      [ ] 3. Triggers

 [#2] Revisar Ponteiros em C          [░░░░░░░░░░]  0% (0/2)
      [ ] 1. Alocação Dinâmica (malloc)
      [ ] 2. Passagem por Referência