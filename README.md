**O projeto foi desenvolvido por Ariadne Silva e Arthur Queiroz**  
**DCA3702 – Algoritmos e Estruturas de Dados II**

# Projeto 1: Modelagem e Análise de Grafos  
**Aplicação de grafos em problemas reais**

---

## 1. Coleta e Preparação dos Dados

Como parte das instruções do trabalho, utilizamos a plataforma de **dados abertos da UFRN** para coletar informações sobre a avaliação dos docentes, disponibilizadas em um arquivo `.csv`.

O ambiente escolhido para o processamento foi o **Google Colab**, onde importamos o arquivo **"avaliacaodocencia.csv"**. A partir disso, iniciamos o tratamento dos dados, realizando:

- Análise das cinco primeiras linhas do conjunto
- Verificação dos nomes e tipos das colunas
- Avaliação da consistência e padronização dos dados

Identificamos que o arquivo contém **12 colunas bem definidas**, entre elas:

- `id_docente`
- `nome_docente`
- `id_turma`
- `ano`

Essas colunas serão fundamentais para a construção da rede de análise baseada em grafos.

### Imagens do tratamento de dados:

- **Imagem 1** – Visualização inicial dos dados  
- **Imagem 2** – Estrutura e tipos das colunas  
- **Imagem 3** – Verificação e limpeza dos dados

---

## 2. Definição do Problema

Com base nos dados analisados, formulamos o seguinte problema:

> **Como os docentes da UFRN se conectam através da coocorrência em turmas?**  
> **É possível identificar uma rede de colaboração com base nos docentes que lecionam para as mesmas turmas?**

Para responder a essas perguntas, utilizaremos as colunas:

- `id_docente`
- `nome_docente`
- `id_turma`

---

## 3. Modelagem da Rede

### Estrutura Escolhida: Grafo de Coocorrência

A estrutura mais adequada para representar esse problema é um **Grafo de Coocorrência**, definido da seguinte forma:

- **Nós (Vértices):**  
  Cada nó representa um docente único, identificado pela coluna `id_docente`.

- **Arestas (Ligações):**  
  Uma aresta será criada entre dois docentes sempre que ambos tiverem lecionado na mesma turma (`id_turma`). Essa ligação representa uma colaboração ou interface de trabalho comum.

### Grafo Ponderado

Para enriquecer a análise, utilizamos um **grafo ponderado**, atribuindo pesos às arestas com base em critérios relevantes:

- **Frequência de Colaboração:**  
  O peso da aresta será o número de turmas compartilhadas entre dois docentes. Arestas com maior peso indicam colaborações mais frequentes.

- **Qualidade da Atuação (alternativa):**  
  Outra possibilidade seria utilizar a média da `atuacao_profissional_media` dos docentes nas turmas compartilhadas. No entanto, nesta etapa, adotamos a **frequência de colaboração** como métrica principal por ser mais direta e eficaz.

---

## 4. Aplicações Estratégicas da Rede de Colaboração

A análise da rede de colaboração entre docentes possui aplicações relevantes em diversos contextos institucionais:

### 4.1 Gestão Acadêmica e Alocação de Recursos

- Identificação de docentes "hubs" para disseminar práticas pedagógicas
- Liderança em reformas curriculares
- Mentoria para novos professores
- Criação de políticas de incentivo à colaboração interdisciplinar

### 4.2 Mapeamento de Comunidades de Prática

- Identificação de clusters coesos e isolados
- Avaliação da integração entre departamentos e campi
- Planejamento de investimentos em infraestrutura colaborativa

### 4.3 Análise de Resiliência da Rede Acadêmica

- Identificação de vulnerabilidades estruturais
- Planejamento de sucessão para docentes centrais
- Programas de distribuição de conhecimento para maior resiliência institucional

### 4.4 Desenvolvimento Profissional dos Docentes

- **Onboarding de Novos Professores:**  
  Facilita a integração de docentes recém-contratados ao identificar parceiros estratégicos.

- **Oportunidades de Pesquisa e Ensino:**  
  Revela parcerias ocultas e potenciais disciplinas interdepartamentais, como a criação de uma disciplina de **Bioestatística** entre Estatística e Biologia.

---

