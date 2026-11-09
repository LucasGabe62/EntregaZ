# 📦 EntregaZ

**Organizador de entregas e prazos acadêmicos.**

Aplicação web simples e rápida para o estudante registrar trabalhos, provas e seminários em poucos segundos e ver, de forma ordenada e destacada por urgência, o que precisa ser feito primeiro.

> Projeto da disciplina de **Inteligência Artificial** — Turma **TADS049** (Senac)
> Autor: **Lucas Gabriel da Silva Santana**

---

## 🎯 Problema

Estudantes recebem trabalhos, provas e seminários por vários canais (sala de aula, portal da faculdade, WhatsApp e e-mail). Sem um lugar único para registrar tudo, é comum esquecer prazos, acumular várias entregas na mesma semana e não saber o que priorizar.

## 💡 Proposta de valor

Diferente de agendas genéricas, o EntregaZ é focado na rotina acadêmica: cada entrega tem **disciplina**, **tipo** e **status**, e a lista é ordenada e colorida conforme a urgência.

---

## ✨ Funcionalidades

- Cadastro, edição e exclusão de entregas (título, disciplina, tipo, data limite e observações)
- Listagem ordenada por data limite, com destaque visual:
  - 🔴 **vermelho**: entregas atrasadas
  - 🟡 **amarelo**: entregas que vencem em até 3 dias
- Filtro por disciplina e por status (pendente, em andamento, concluída)
- Marcação de entrega como concluída
- Painel-resumo com o total de entregas pendentes, atrasadas e concluídas

### Fora do escopo desta versão

- Login e múltiplos usuários (uso individual e local)
- Notificações por e-mail, push ou WhatsApp
- Integração com portais acadêmicos ou calendários externos (ex.: Google Agenda)
- Aplicativo mobile nativo (a interface é apenas responsiva)
- Compartilhamento de entregas entre colegas

---

## 🛠️ Tecnologias

| Camada | Tecnologia |
|---|---|
| Frontend | HTML, CSS e JavaScript puro (layout responsivo) |
| Backend | Python + FastAPI (API REST) |
| Banco de dados | SQLite |

**Por que essas escolhas?**
O FastAPI permite criar uma API clara, validar dados com poucas linhas e gera documentação automática dos endpoints. O SQLite dispensa servidor de banco de dados e atende bem a um sistema de uso individual. HTML/CSS/JS puro mantém o frontend leve e sem dependências, adequado ao tamanho do projeto.

### Arquitetura

```
Navegador (HTML/CSS/JS) → HTTP/JSON → API FastAPI (regras de negócio e validação) → SQLite
```

### Modelo de dados

Tabela `entregas`:

| Campo | Descrição |
|---|---|
| `id` | Identificador da entrega |
| `titulo` | Título da entrega |
| `disciplina` | Disciplina relacionada |
| `tipo` | trabalho, prova, seminário ou outro |
| `data_limite` | Data limite de entrega |
| `observacoes` | Observações (opcional) |
| `status` | pendente, em andamento ou concluída |
| `criada_em` | Data/hora de criação do registro |

---

## 🚀 Como executar

> ⚠️ Ajuste os nomes de pastas e arquivos conforme a estrutura real do seu repositório.

```bash
# 1. Clone o repositório
git clone https://github.com/<seu-usuario>/<seu-repositorio>.git
cd <seu-repositorio>

# 2. (Opcional) Crie e ative um ambiente virtual
python -m venv venv
source venv/bin/activate        # Linux/macOS
venv\Scripts\activate           # Windows

# 3. Instale as dependências
pip install -r requirements.txt

# 4. Inicie a API
uvicorn main:app --reload
```

Depois, acesse:

- Aplicação: `http://localhost:8000`
- Documentação automática da API: `http://localhost:8000/docs`

---

## 📋 Requisitos

### Histórias de usuário

| ID | História |
|---|---|
| HU01 | Como estudante, quero cadastrar uma entrega com disciplina e data limite, para que eu não esqueça nenhum prazo. |
| HU02 | Como estudante, quero ver minhas entregas ordenadas por data e com as atrasadas destacadas, para que eu saiba o que priorizar hoje. |
| HU03 | Como estudante, quero filtrar as entregas por disciplina, para que eu consiga planejar os estudos de cada matéria. |
| HU04 | Como estudante, quero marcar uma entrega como concluída, para que minha lista mostre apenas o que ainda falta fazer. |
| HU05 | Como estudante, quero editar ou excluir uma entrega, para que eu corrija datas que mudaram ou remova registros errados. |

### Requisitos funcionais

| ID | Descrição |
|---|---|
| RF01 | Cadastrar entrega com título, disciplina, tipo (trabalho, prova, seminário, outro), data limite e observações opcionais. |
| RF02 | Listar entregas por data limite crescente, destacando em vermelho as atrasadas e em amarelo as que vencem em até 3 dias. |
| RF03 | Filtrar a listagem por disciplina e por status. |
| RF04 | Editar, excluir e marcar uma entrega como concluída. |
| RF05 | Exibir resumo com a quantidade de entregas pendentes, atrasadas e concluídas. |

### Requisitos não funcionais

| ID | Categoria | Descrição |
|---|---|---|
| RNF01 | Desempenho | Listagem e filtros respondem em até 1 segundo com até 500 entregas. |
| RNF02 | Usabilidade | Interface responsiva, utilizável a partir de 360 px de largura; cadastro em no máximo uma tela de formulário. |
| RNF03 | Persistência | Dados em SQLite, sem perda de informação ao reiniciar o sistema. |
| RNF04 | Compatibilidade | Funciona nas versões atuais do Chrome, Firefox e Edge. |
| RNF05 | Manutenibilidade | Código separa interface, regras de negócio e acesso a dados. |

---

## 🧩 Caso de uso principal: UC01 – Cadastrar nova entrega

**Ator:** Estudante
**Pré-condição:** o sistema está aberto na tela principal.

### Fluxo principal

1. **Entrada:** o estudante clica em "Nova entrega", preenche título, disciplina, tipo e data limite (observações são opcionais) e clica em "Salvar".
2. **Processamento:** o sistema valida os campos obrigatórios, confere se a data limite é válida, grava a entrega com status "pendente" e recalcula o resumo.
3. **Retorno:** o sistema exibe "Entrega cadastrada com sucesso", fecha o formulário e mostra a lista atualizada, com a nova entrega na posição correta.

### Fluxos de exceção

| ID | Situação | Comportamento do sistema |
|---|---|---|
| E1 | Campo obrigatório vazio | Não grava nada, destaca o campo e exibe "Preencha o título, a disciplina e a data limite". |
| E2 | Data inválida ou no passado | Data no passado: pede confirmação ("Esta data já passou. Deseja cadastrar mesmo assim?"). Formato inválido: rejeita. |
| E3 | Falha ao gravar no banco | Mantém os dados no formulário, exibe "Não foi possível salvar. Tente novamente" e registra o erro em log. |
| E4 | Entrega não encontrada (editar/excluir) | Exibe "Entrega não encontrada" e atualiza a lista. |

---

## ✅ Critérios de aceite (Definição de Pronto)

- [ ] Cadastrar, listar, editar, excluir e concluir entregas sem travamentos ou erros não tratados (RF01 a RF04)
- [ ] Validação impede cadastro com título, disciplina ou data limite vazios, com mensagens de erro claras
- [ ] Lista ordenada por data limite, com atrasadas em vermelho e as que vencem em até 3 dias em amarelo
- [ ] Filtros por disciplina e status retornam resultados corretos
- [ ] Painel-resumo exibe os totais corretos de pendentes, atrasadas e concluídas
- [ ] Dados permanecem salvos após fechar e reabrir o sistema
- [ ] Interface funciona em tela de celular (360 px) e nos navegadores Chrome, Firefox e Edge
- [ ] Todos os fluxos de exceção (E1 a E4) testados manualmente
- [ ] Código versionado em Git, com README explicando como executar o projeto
