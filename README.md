# Toxicity Analyzer — Análise de Perfis do Reddit

Uma aplicação com interface Streamlit que permite analisar perfis de usuários do Reddit, identificando comentários tóxicos e odiosos através de um modelo de machine learning treinado localmente.

## Sumário

- [Visão Geral](#visão-geral)
- [Arquitetura](#arquitetura)
- [Decisões de Design](#decisões-de-design)
- [Pipeline Detalhada](#pipeline-detalhada)
- [Como Executar a Aplicação](#como-executar-a-aplicação)
- [Como Usar a Aplicação](#como-usar-a-aplicação)
- [Limitações Conhecidas e Próximos Passos](#limitações-conhecidas-e-próximos-passos)
- [Dicionário para Commits](#dicionário-para-commits)

## Considerações Iniciais

Este projeto foi implementado com foco na análise de toxicidade em comentários do Reddit, utilizando um modelo de machine learning treinado para identificar discurso de ódio. A aplicação oferece uma interface amigável e intuitiva para visualizar os resultados da análise, incluindo gráficos e métricas relevantes.

## Visão Geral

Este projeto demonstra uma aplicação de análise de toxicidade para perfis do Reddit: o usuário insere um nome de usuário do Reddit, o sistema coleta os comentários públicos desse usuário, analisa cada comentário com um modelo de machine learning pré-treinado e apresenta os resultados em uma interface gráfica interativa.

- **Frontend**: Streamlit com UI moderna e responsiva, incluindo gráficos interativos e métricas visuais.
- **Análise**: Modelo de machine learning treinado para detectar discurso de ódio em comentários.
- **Visualização**: Gráficos e métricas que mostram a distribuição de comentários tóxicos, atividade em subreddits e histórico temporal.
- **API Reddit**: Integração com a API do Reddit via PRAW para coleta de dados.

## Arquitetura

```
[Input do Username]
     │
     ▼
[Coleta de Comentários via PRAW]
     │  extrai comentários públicos do usuário
     ▼
[Pré-processamento]
     │  prepara o texto para análise
     ▼
[Vetorização]
     │  transforma texto em vetores numéricos
     ▼
[Modelo de Machine Learning]
     │  classifica comentários como tóxicos ou não
     ▼
[Cálculo de Métricas]
     │  percentual de toxicidade, distribuição, etc.
     ▼
[Visualização em Streamlit]
     │  apresentação dos resultados em interface gráfica
     ▼
[Relatório Final: métricas + gráficos + comentários mais tóxicos]
```

- **streamlit_app.py**: UI, estado de sessão, integração com API do Reddit, análise de comentários, visualização dos resultados.
- **data_setup.py**: Processamento e preparação dos dados para treinamento do modelo.
- **joblib/**: Diretório contendo o modelo treinado e o vetorizador.

## Decisões de Design

- **Modelo Local**: Modelo de machine learning treinado localmente e armazenado em formato joblib, permitindo análise rápida sem dependência de APIs externas.
- **Interface Moderna**: Design glassmorphism com cores contrastantes para melhor visualização dos resultados.
- **Gráficos Interativos**: Utilização da biblioteca Plotly para criar visualizações interativas e informativas.
- **Análise Limitada**: Opção para definir o número máximo de comentários a serem analisados, equilibrando tempo de processamento e abrangência da análise.
- **Métricas Claras**: Apresentação de métricas objetivas como percentual de comentários tóxicos e classificação do perfil.

## Pipeline Detalhada

### Coleta de Dados

- Utiliza a biblioteca PRAW para acessar a API do Reddit.
- Coleta os comentários mais recentes do usuário especificado.
- Filtra comentários deletados ou removidos.

### Análise de Toxicidade

- Pré-processamento do texto (conversão para minúsculas).
- Vetorização do texto utilizando o vetorizador pré-treinado.
- Classificação binária (tóxico/não-tóxico) utilizando o modelo pré-treinado.
- Cálculo da probabilidade de toxicidade para cada comentário.

### Visualização dos Resultados

- Métricas gerais: total de comentários, comentários tóxicos, percentual de toxicidade.
- Classificação do perfil: de "Perfil Limpo" a "Extremamente Tóxico".
- Gráficos: distribuição de comentários tóxicos, atividade em subreddits, histórico temporal.
- Lista dos comentários mais tóxicos com detalhes e links.

## Como Executar a Aplicação

### Acesso Online

A aplicação está disponível online através do Streamlit Cloud, não sendo necessário executá-la localmente:

**[https://reddit-toxicity-analyzer.streamlit.app/](https://reddit-toxicity-analyzer.streamlit.app/)**

Basta acessar o link acima para utilizar a aplicação diretamente no navegador.

### Execução Local (Opcional)

Caso deseje executar a aplicação localmente:

1. Clone o repositório:
   ```
   git clone https://github.com/seu-usuario/toxicity-analyzer.git
   cd toxicity-analyzer
   ```

2. Instale as dependências:
   ```
   pip install -r requirements.txt
   ```

3. Configure as credenciais da API do Reddit:
   - Crie um arquivo `.env` na raiz do projeto com as seguintes variáveis:
     ```
     CLIENT_ID=seu_client_id
     CLIENT_SECRET=seu_client_secret
     USER_AGENT=seu_user_agent
     ```
   - Para obter essas credenciais, crie uma aplicação em https://www.reddit.com/prefs/apps

4. Execute a aplicação:
   ```
   streamlit run streamlit_app.py
   ```

## Como Usar a Aplicação

1. Acesse a aplicação no navegador (geralmente em http://localhost:8501).
2. Digite o nome de usuário do Reddit que deseja analisar.
3. Ajuste o limite de comentários a serem analisados (10-500).
4. Clique no botão "ANALISAR".
5. Visualize os resultados:
   - Métricas gerais e classificação do perfil.
   - Gráficos de distribuição e atividade.
   - Lista dos comentários mais tóxicos.

## Limitações Conhecidas e Próximos Passos

- **Limitação da API**: A API do Reddit limita o número de requisições por minuto, o que pode afetar a análise de múltiplos usuários em sequência.
- **Modelo de Classificação**: O modelo atual é binário (tóxico/não-tóxico) e poderia ser expandido para classificar diferentes tipos de toxicidade.
- **Análise Temporal**: Implementar análise de tendências ao longo do tempo para verificar mudanças no comportamento do usuário.
- **Exportação de Resultados**: Adicionar opção para exportar os resultados em formato CSV ou PDF.
- **Análise de Subreddits**: Expandir a análise para incluir a toxicidade média em diferentes subreddits.

## Dicionário para Commits

| Letra | Tipo     | Descrição                                         | Exemplo                                                |
| ----- | -------- | ------------------------------------------------- | ------------------------------------------------------ |
| **F** | Feature  | Nova funcionalidade                               | `F: adicionar pipeline de análise de toxicidade`       |
| **B** | Bug      | Correção de bug                                   | `B: corrigir erro na função de limpeza de comentários` |
| **R** | Refactor | Refatoração de código, sem alterar funcionalidade | `R: otimizar função de pré-processamento`              |
| **D** | Docs     | Documentação ou comentários                       | `D: atualizar README com instruções de uso`            |
| **C** | Chore    | Tarefas gerais, configuração ou manutenção        | `C: atualizar dependências e .env`                     |
| **X** | Data     | Alterações em datasets ou pré-processamento       | `X: aplicar undersampling e limpeza de @USER`          |
