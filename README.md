====================================================================================

# RESUMO: DATA MART DE VIGILÂNCIA EPIDEMIOLÓGICA DE ARBOVIROSES
Projeto da disciplina de Sistemas de Apoio a Decisão, usando Dados Abertos do SINAN

====================================================================================

---

## CONTEXTUALIZACAO E OBJETIVO
O projeto consiste na modelagem dimensional e implementacao de um Data Mart 
voltado para a analise integrada de casos de arboviroses (Dengue, Zika e 
Chikungunya) no Brasil, abrangendo o periodo recente de 2024 a 2025. O 
principal objetivo e fornecer uma infraestrutura de dados otimizada para 
apoiar a vigilancia epidemiologica, o monitoramento de surtos, a geracao 
de relatorios gerenciais e paineis interativos para gestores de saude publica.

---

## ARQUITETURA TECNOLOGICA E CAMADAS
A solucao esta estruturada em uma arquitetura de 5 camadas:

- Fontes de Dados: Arquivos CSV brutos extraidos do portal de Dados Abertos do Ministerio da Saude (SINAN).
- ETL (PDI): Fluxos desenvolvidos no Pentaho Data Integration (.ktr) responsaveis pela extracao, tratamento de valores nulos, mapeamento de codigos (CID-10, raca/cor, sexo) e carga.
- Data Mart (PostgreSQL): Banco de dados relacional estruturado sob o modelo de Esquema Estrela (Star Schema) no SGBD PostgreSQL (arboviroses_db).
- Camada Semantica/Servidor OLAP: Otimizacao para consultas multidimensionais de alta performance.
- Power BI: Aplicacao de visualizacao executiva contendo cartões de KPI, mapas geograficos e graficos analíticos de sazonalidade e perfil demografico.

---

## MODELAGEM DIMENSIONAL (STAR SCHEMA)
O modelo relacional/dimensional foca na tabela fato central rodeada por quatro dimensoes:

- Tabela Fato (fato_arboviroses_estrelar): Armazena as chaves estrangeiras (pk_tempo, pk_paciente, pk_localidade, pk_doenca) e a metrica aditiva de contagem (total_notificacao, equivalente a 1 por registro de notificacao). Estima-se 1 milhao de registros.
- Dimensao Tempo (dim_tempo): Cobre cerca de 730 dias (2 anos), contendo atributos como data de notificacao, dia, mes (numero e nome) e ano calendario (Hierarquia: Ano -> Mes).
- Dimensao Localidade (dim_localidade): Contém os atributos geograficos com codigo IBGE, municipio e UF (Hierarquia: Estado -> Municipio).
- Dimensao Paciente (dim_paciente): Segmentacao demografica por sexo biologico, raca/cor e idade.
- Dimensao Doenca (dim_doenca): Classificacao dos agravos: Dengue, Zika e Chikungunya.

---

## INDICADORES DE NEGOCIO E CONSULTAS OLAP
O painel gerencial implementado responde a perguntas estrategicas fundamentais para a tomada de decisao em saude publica:

- Total de Casos (Volume Absoluto): Mensuracao da magnitude geral da epidemia para dimensionamento de insumos medicos e recursos financeiros.
- Variacao Percentual Anual (YoY): Comparativo percentual entre periodos (ex: 2025 vs. 2024) para avaliar tendencias de aceleracao ou controle de endemias.
- Sazonalidade e Mes de Pico: Identificacao estatistica do periodo de maior transmissao (com pico historico mapeado em abril) para planejar o reforco de equipes de atendimento.
- Perfil Demografico (Genero e Faixa Etaria): Monitoramento de grupos vulneraveis (como a concentracao em publico adulto e idosos, alem de analises pediatricas).
- Ranking de Municipios Criticos: Ordenacao por volume de casos para priorizar mutiroes de limpeza urbana e repasses de verbas emergenciais.
