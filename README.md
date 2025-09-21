**O projeto foi desenvolvido por Ariadne Silva e Arthur Queiroz**  
**DCA3702 – Algoritmos e Estruturas de Dados II**

# Projeto 1: Modelagem e Análise de Grafos  
**Aplicação de grafos em problemas reais**

---

## 1. Coleta e Preparação dos Dados

Como parte das instruções do trabalho, utilizamos a plataforma de **dados abertos da UFRN** para coletar informações sobre a avaliação dos docentes, disponibilizadas em um arquivo `.csv`.

O ambiente escolhido para o processamento foi o **Google Colab**, onde importamos o arquivo **"avaliacaodocencia.csv"**. Partindo do uso da bibliotecas pandas e o arquivo `.csv`, iniciamos o tratamento dos dados, realizando:

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

## 5. Construção do Grafo a partir da Base de Dados 

Nesta etapa, implementamos em Python — utilizando a biblioteca `networkx` — a estrutura de grafo ponderado que representa nossa rede. Os **nós** correspondem aos **docentes**, enquanto as **arestas** indicam a **coocorrência desses docentes em turmas compartilhadas**. Essa transformação permite representar os dados de forma relacional, viabilizando análises mais profundas sobre a dinâmica entre os professores.

## Transformação da base de dados em um grafo

```python
print("Iniciando a Etapa: Transformação da base de dados em um grafo...")

# 1. Criar um grafo vazio (não-direcionado)
G = nx.Graph()

# 2. Adicionar os docentes como nós
todos_docentes_ids = df['id_docente'].unique()
G.add_nodes_from(todos_docentes_ids)

print(f"Foram adicionados {G.number_of_nodes()} nós (docentes) ao grafo.")

# 3. Criar arestas a partir das turmas compartilhadas
turmas_agrupadas = df.groupby('id_turma')['id_docente'].apply(list)

for turma_id, docentes_na_turma in turmas_agrupadas.items():
    if len(docentes_na_turma) > 1:
        pares_de_docentes = combinations(docentes_na_turma, 2)
        
        for docente1, docente2 in pares_de_docentes:
            if G.has_edge(docente1, docente2):
                G[docente1][docente2]['weight'] += 1  # Aumenta o peso da colaboração
            else:
                G.add_edge(docente1, docente2, weight=1)  # Nova colaboração

print(f"Foram adicionadas {G.number_of_edges()} arestas (colaborações) ao grafo.")
print("\n Etapa concluído: Grafo de coocorrência ponderado criado com sucesso!")
```


### Saída

```text
Iniciando a Etapa: Transformação da base de dados em um grafo...

Foram adicionados 5963 nós (docentes) ao grafo.

Foram adicionadas 25922 arestas (colaborações) ao grafo.

Etapa concluído: O grafo de coocorrência ponderado foi criado com sucesso!
```
---

## 6. Análise da Rede 

Com o grafo modelado, partimos para a extração de métricas e insights conforme os objetivos do projeto. Utilizamos conceitos da teoria de grafos para calcular propriedades fundamentais da rede, como:

- **Número total de nós** (docentes)
- **Número total de arestas** (coocorrências)
- **Grau de cada nó**
- **Grau médio da rede**
- **Densidade da rede**

Essas métricas oferecem uma visão inicial sobre a estrutura e conectividade da rede docente, servindo como base para análises mais avançadas.

## Etapa: Análise das propriedades do grafo

### Código
```python
print("Iniciando a Etapa: Análise das propriedades do grafo...")

# 1. Métricas Básicas
num_nos = G.number_of_nodes()
num_arestas = G.number_of_edges()

print(f"\n- Número de Nós (docentes): {num_nos}")
print(f"- Número de Arestas (colaborações): {num_arestas}")

# 2. Métricas de Grau
graus = [val for (node, val) in G.degree()]

if num_nos > 0:
    grau_medio = sum(graus) / num_nos
    print(f"\n- Grau Médio da Rede: {grau_medio:.2f}")

    docente_mais_conectado = max(G.degree(), key=lambda item: item[1])
    docente_mais_conectado_id = docente_mais_conectado[0]
    grau_maximo = docente_mais_conectado[1]

    print(f"- Docente mais conectado: ID {docente_mais_conectado_id} com {grau_maximo} conexões.")
else:
    print("\n- Não é possível calcular métricas de grau, grafo vazio.")

# 3. Densidade da Rede
densidade = nx.density(G)
print(f"\n- Densidade da Rede: {densidade:.6f}")

# 4. Componentes Conectados
num_componentes = nx.number_connected_components(G)
print(f"\n- Número de Componentes Conectados: {num_componentes}")

if num_componentes > 0:
    componentes = list(nx.connected_components(G))
    maior_componente = max(componentes, key=len)
    print(f"- O maior componente conectado possui {len(maior_componente)} docentes.")

    G_maior_componente = G.subgraph(maior_componente).copy()

print("\n Etapa concluído: A análise inicial do grafo foi realizada com sucesso!")
```
### Saída

```text
Iniciando a Etapa Análise das propriedades do grafo...

- Número de Nós (docentes): 5963
- Número de Arestas (colaborações): 25922

- Grau Médio da Rede: 8.69
- Docente mais conectado: ID 25946 com 164 conexões.

- Densidade da Rede: 0.001458

- Número de Componentes Conectados: 1395
- O maior componente conectado possui 3960 docentes.

Etapa concluído: A análise inicial do grafo foi realizada com sucesso!
```

### Interpretação dos Resultados  

O grafo obtido revela informações importantes sobre a rede de colaboração docente. O número de nós e arestas confirma a quantidade de docentes e colaborações existentes entre eles. O grau médio da rede indica com quantos outros docentes, em média, cada professor leciona na mesma turma; neste caso, aproximadamente nove, o que representa uma medida relevante da interação acadêmica.  

O docente mais conectado (ID 25946) aparece como um verdadeiro “hub” da rede, com 164 colaborações. Esse professor desempenha um papel central, possivelmente por ministrar disciplinas compartilhadas entre diversos cursos ou departamentos. A densidade da rede, com valor de 0.001458, é muito próxima de zero, o que mostra que a rede é esparsa. Esse comportamento é típico em grandes redes sociais ou de colaboração, nas quais a maioria dos indivíduos não está diretamente conectada com todos os demais.  

Outro aspecto relevante é a análise dos componentes conectados. Cada componente pode ser entendido como um subgrupo de docentes no qual existe um caminho de colaboração entre quaisquer dois professores. Em termos práticos, eles funcionam como “ilhas” de colaboração. O maior componente, considerado o “continente”, reúne o núcleo principal da rede na UFRN e conecta 3.960 docentes de forma direta ou indireta. Já os outros 1.394 componentes representam grupos menores e isolados, como núcleos de pesquisa específicos, professores de programas de pós-graduação que só lecionam entre si, departamentos mais fechados ou ainda campi do interior que colaboram internamente, mas não interagem com o restante da instituição.  

A análise evidencia que, embora exista um grande grupo central altamente interconectado, a rede também apresenta uma fragmentação significativa, composta por diversos grupos menores que não se conectam à estrutura principal.  

---

## 7. Visualização do Grafo

### Visualização da Rede de Colaboração  

A partir deste momento, buscamos transformar os números e métricas calculados em uma representação visual e intuitiva da rede de colaboração, utilizando bibliotecas como `NetworkX`, `Matplotlib` e `Plotly` para criar visualizações interativas.  

Na primeira tentativa, procuramos visualizar o grafo completo, com quase 6.000 nós. O resultado, porém, foi uma imagem extremamente poluída e de difícil interpretação, além de ser apenas estática.  

<p align="center">
  <img src="./imagem/grafo01.png" alt="QR code com o link do projeto"><br>
</p>

Na segunda abordagem, experimentamos criar uma **ego network**, isto é, uma rede centrada no docente mais conectado, por representar a parte mais significativa da estrutura. O nó central foi destacado com cor e tamanho diferentes para facilitar a compreensão. Ainda assim, o resultado apresentou limitações: uma rede densa, com sobreposição de nós, ausência de interatividade e pouca possibilidade de detalhamento para análises mais profundas.  

Em uma terceira tentativa, voltamos nosso foco para o **maior componente conectado da rede**, que reúne 3.960 docentes. Essa escolha fazia sentido, pois corresponde ao “coração” da rede de colaboração. No entanto, tentar desenhar um grafo dessa dimensão comprometeu o desempenho, tornando o processo custoso e pouco eficiente. Para amenizar o problema, reduzimos o número de iterações do algoritmo de layout para cerca de 50 e ajustamos parâmetros visando diminuir a poluição visual. Apesar do esforço, o resultado continuou insatisfatório: a rede se mostrou como uma massa densa e emaranhada de nós e arestas, inviabilizando a identificação de docentes, a distinção de clusters ou comunidades e a compreensão da estrutura interna.  

Dessa forma, concluímos que, para redes de grande porte, a visualização estática não é suficiente para atender ao objetivo de interpretar os resultados de maneira clara e criativa. Isso levou a duas mudanças fundamentais na solução final: **a troca da ferramenta**, adotando o `Plotly` para possibilitar interatividade, e **a redefinição do escopo**, restringindo o foco a um subconjunto ainda mais relevante — os 100 docentes mais centrais, que representam a verdadeira “elite” da colaboração. 

---

## 7. Resultados

A visualização apresenta a rede dos **100 docentes mais conectados** da universidade, representada por um gráfico de dispersão. Cada círculo (nó) corresponde a um professor, e a proximidade entre os nós reflete a intensidade de suas conexões. A variação de **cor** e **tamanho** dos nós, indicada pela legenda *“Número de Conexões”*, ajuda a identificar a importância relativa de cada docente dentro da rede.  

Ao observar o gráfico, percebemos que a rede não forma um único bloco uniforme. Em vez disso, surgem **três agrupamentos distintos**, cada um com características próprias que contam uma história sobre a colaboração acadêmica.

O **Cluster Principal**, localizado no canto inferior direito, é o maior e mais denso. Seus nós são maiores e de cor azul intenso, representando os docentes com mais colaborações, chegando a **60 conexões**. Este grupo pode ser entendido como o **núcleo central de colaboração da universidade**, composto por professores de departamentos grandes ou que lecionam disciplinas compartilhadas por diversos cursos, mantendo a coesão e o fluxo de informação dentro da instituição.

Mais à esquerda, encontramos o **Cluster Secundário**, menor e menos denso, com nós verde-claro e entre 10 e 20 conexões. Este agrupamento sugere a presença de um **departamento ou centro acadêmico coeso**, que colabora intensamente entre si, mas tem menos interações com o núcleo principal. Ele revela que, além do grande núcleo central, existem comunidades menores que mantêm forte colaboração interna, mas isoladas do fluxo principal.

No canto superior esquerdo, observa-se um **nó isolado**, de cor amarela-clara, quase desconectado da rede. Apesar de estar entre os 100 docentes mais centrais, este professor pertence a uma **comunidade paralela**, destacando a existência de componentes isolados que colaboram pouco com os principais hubs.

A interpretação da cor e do tamanho dos nós revela ainda uma **hierarquia dentro da rede**. Os nós azul-escuro do cluster principal representam os **docentes hub**, verdadeiros superconectores, essenciais para a estrutura e coesão da rede. Já os nós de tamanho e cor intermediários, localizados na periferia ou entre clusters, funcionam como **docentes ponte**, conectando diferentes grupos e facilitando a circulação de conhecimento entre comunidades distintas.

Em conjunto, essa visualização permite compreender não apenas quem são os docentes mais conectados, mas também como se organizam as relações de colaboração na universidade, revelando **núcleos centrais, grupos internos coesos e componentes isolados**, oferecendo uma perspectiva rica e interpretativa da estrutura acadêmica.

---

## 8. Conclusão

A questão inicial do estudo foi: *"Como os docentes da UFRN se conectam através da coocorrência em turmas? É possível identificar uma rede de colaboração com base nos docentes que lecionam para as mesmas turmas?"*  

A resposta é **sim**. A análise realizada mostra que os docentes efetivamente se conectam, formando padrões de colaboração que podem ser mapeados e interpretados. Esses padrões não são uniformes: a rede é **esparsa e fragmentada**, o que significa que, embora existam colaborações, a maioria dos docentes não interage diretamente com todos os outros. Muitos grupos funcionam de forma isolada, enquanto outros formam núcleos mais densos de interação.  

No entanto, mesmo dentro dessa fragmentação, identifica-se um **núcleo central de colaboração**: o maior componente conectado da rede reúne **3.960 docentes**, funcionando como o coração da rede. Neste núcleo, os professores estão interligados direta ou indiretamente, garantindo a coesão e o fluxo de informações acadêmicas. A presença de **hubs**, como docentes com dezenas ou mesmo centenas de conexões, evidencia a existência de atores-chave que sustentam a rede e conectam diferentes grupos. Além disso, a visualização revelou clusters distintos e até nós isolados, mostrando que a colaboração ocorre em comunidades menores e especializadas, provavelmente alinhadas a departamentos, centros acadêmicos ou áreas do conhecimento.

A validação da rede confirma que a modelagem foi bem-sucedida. A partir de dados tabulares, transformamos os registros de turmas em um **grafo de coocorrência ponderado**, com **5.963 nós** e **25.922 arestas**. As métricas quantitativas — número de nós, arestas, densidade, grau médio e identificação de hubs — fornecem evidências concretas da estrutura da rede. Por fim, a **visualização interativa dos 100 docentes mais centrais** demonstra, de forma clara e intuitiva, como essas conexões se organizam, permitindo a análise de clusters, hubs e pontes entre grupos.

Em suma, o estudo não apenas responde afirmativamente à pergunta inicial, mas também oferece uma compreensão detalhada da **estrutura, escala e dinâmica da colaboração docente** na UFRN, destacando os principais atores, os clusters e o núcleo central que sustentam a rede.

