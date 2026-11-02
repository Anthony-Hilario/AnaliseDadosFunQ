# Análise de Dados do FunQ: Jogo Educacional sobre Pensamento Computacional

Repositório com os dados e o notebook de análise utilizados no Trabalho de Conclusão de Curso **"AVALIAFUNQ: PLATAFORMA WEB PARA MENSURAÇÃO QUALIQUANTITATIVA DA EXPERIÊNCIA LÚDICA DE ESTUDANTES COM SERIOUS GAMES
"**, do curso de Tecnologia em Sistemas para Internet do IFRN (Campus Currais Novos).

A plataforma web aplica o *Fun Questionnaire* (FunQ) com mais algumas questões abertas aos estudantes após a atividade com o jogo de tabuleiro desplugado. Este repositório reúne a análise descritiva das respostas coletadas.

---

## ⚠️ Aviso importante: dados simulados (mock)

**Todos os dados deste repositório são fictícios**, gerados apenas para demonstrar que a plataforma e o fluxo de análise funcionam. Eles **não representam respostas reais de estudantes** e não devem ser interpretados como resultados empíricos.

Os registros foram simulados originalmente como se pertencessem a duas turmas. Como o objetivo do trabalho é descritivo e não comparativo, a análise considera os **40 alunos como uma única turma**.

---

## Estrutura do repositório

| Arquivo | Descrição |
|---|---|
| `analise-respostas-funq.ipynb` | Notebook Jupyter com toda a análise e os gráficos |
| `respostas_formulario.csv` | Respostas simuladas do FunQ (uma linha por aluno), identificadas por `aluno_id` |
| `alunos.csv` | Cadastro simulado dos 40 alunos, com apenas as colunas `id` e `idade` |
| `README.md` | Este arquivo |

### Sobre os CSVs

- **`alunos.csv`**: 40 registros com `id` e `idade`. Não há nomes nem qualquer outro dado pessoal.
- **`respostas_formulario.csv`**: contém `aluno_id` e as respostas às 18 questões da escala FunQ e as respostas abertas (`q1` a `q18`, notas de 1 a 5; `q19` a `q24`, discursivas).
- A ligação entre as duas tabelas é feita por `respostas_formulario.csv → aluno_id` ↔ `alunos.csv → id`. No notebook, as tabelas são mantidas em variáveis separadas (`df` e `df_alunos`), e a idade é trazida para uma tabela temporária (`df_idade`) apenas nas análises que dependem dela.

---

## Como o FunQ é calculado

O FunQ possui 18 itens, respondidos em uma **escala de frequência de 1 a 5** (1 = Nunca, 5 = O tempo todo), distribuídos em 6 dimensões de 3 itens cada:

| Dimensão | Itens |
|---|---|
| Autonomia (A) | q1, q2, q3 |
| Desafio (C) | q4, q5, q6 |
| Prazer (D) | q7, q8, q9 |
| Imersão (I) | q10, q11, q12 |
| Perda de Barreiras Sociais (SB) | q13, q14, q15 |
| Estresse (S) | q16, q17, q18 |

**Itens invertidos [R]:** alguns itens têm pontuação inversa e são recodificados antes do cálculo, pela fórmula `nota_invertida = 6 - nota` (1 vira 5, 2 vira 4, 3 permanece 3, 4 vira 2 e 5 vira 1). Os itens invertidos são q2, q16, q17 e q18.

**Pontuação de cada dimensão:** média simples dos três itens (já com a inversão aplicada), por aluno. Exemplo: `Desafio = (q4 + q5 + q6) / 3`.

Instrumento original: TISZA, Gabriella; MARKOPOULOS, Panos. The Fun Questionnaire (FunQ): Measuring fun in learning with adolescents. *Computers & Education*, v. 195, p. 104712, 2023.

---

## O que o notebook faz

O notebook `analise-respostas-funq.ipynb` deve ser executado **na ordem das células**, pois as etapas posteriores dependem das variáveis criadas nas anteriores:

1. **Carregamento dos dados:** leitura de `respostas_formulario.csv` e `alunos.csv`.
2. **Cálculo das dimensões:** inversão dos itens [R] e média por dimensão.
3. **Médias por dimensão:** gráfico de barras com o escore médio da turma.
4. **Estatística descritiva:** mediana e IQR, desvio padrão, mínimo, máximo e amplitude de cada dimensão.
5. **Correlação entre dimensões:** matriz de correlação (heatmap), incluindo relações como Desafio × Imersão e Estresse × Prazer.
6. **Idade como variável exploratória:** dispersão de cada dimensão por idade e comparação entre faixas etárias (14-15 e 16-17 anos).

---

## Como executar

**Requisitos:** Python 3 e as bibliotecas abaixo.

```bash
pip install pandas matplotlib seaborn jupyter
```

**Passos:**

```bash
git clone <url-do-repositorio>
cd <pasta-do-repositorio>
jupyter notebook analise-respostas-funq.ipynb
```

Mantenha os dois arquivos CSV na **mesma pasta** do notebook, pois ele os lê pelo nome (`pd.read_csv('alunos.csv')` e `pd.read_csv('respostas_formulario.csv')`).

---

## Guia para não-programadores

Esta seção é para professores e demais interessados que querem usar os arquivos sem experiência com programação. Há três caminhos, do mais simples ao mais completo.

### Caminho 1: apenas ver os dados (sem instalar nada)

Os arquivos `.csv` são tabelas simples e podem ser abertos no **Excel**, no **LibreOffice Calc** ou no **Google Planilhas** (Arquivo → Importar → Upload).

- `alunos.csv`: lista de alunos com `id` e `idade`.
- `respostas_formulario.csv`: cada linha é um aluno e cada coluna `q1` a `q18` é uma pergunta do questionário (notas de 1 a 5).

Se quiser apenas conferir as médias por dimensão de cada aluno, a tela de administração da plataforma já mostra essa tabela pronta.

### Caminho 2: ver os gráficos e rodar a análise pelo navegador (recomendado)

Usa o **Google Colab**, que roda o notebook online, sem instalar nada. É necessária uma conta Google.

1. Acesse [colab.research.google.com](https://colab.research.google.com) e entre com sua conta Google.
2. Clique em **Arquivo → Fazer upload de notebook** e selecione `analise-respostas-funq.ipynb`.
3. No menu lateral esquerdo, clique no **ícone de pasta** e depois no ícone de **upload**. Envie os dois arquivos `alunos.csv` e `respostas_formulario.csv`.
4. Clique em **Ambiente de execução → Executar tudo**.
5. Role a página: os gráficos e tabelas aparecem abaixo de cada célula.

Observações:
- Os dois CSVs precisam ser enviados ao Colab **antes** de executar, porque o notebook os procura pelo nome.
- O Colab já traz `pandas`, `matplotlib` e `seaborn` instalados.
- Os arquivos enviados ao Colab somem quando a sessão termina. Basta repetir o passo 3 na próxima vez.

### Caminho 3: rodar no próprio computador

1. Instale o **Anaconda** ([anaconda.com/download](https://www.anaconda.com/download)), que já inclui Python, Jupyter e as bibliotecas necessárias.
2. Baixe este repositório (botão **Code → Download ZIP** no GitHub) e extraia a pasta.
3. Abra o **Anaconda Navigator**, inicie o **Jupyter Notebook** e navegue até a pasta extraída.
4. Abra `analise-respostas-funq.ipynb` e clique em **Run → Run All Cells** (ou execute célula por célula com `Shift + Enter`).

### Como ler os resultados

- Todas as notas vão de **1 a 5**.
- Depois da inversão dos itens [R], **valores mais altos indicam uma experiência mais favorável em todas as dimensões**, inclusive em Estresse (nota alta = menos estresse) e em Desafio (nota alta = desafio adequado, nem fácil nem difícil demais).
- **Média e mediana** mostram o resultado típico da turma. **Desvio padrão, IQR e amplitude** mostram o quanto as respostas variam entre os alunos.
- Lembre-se: os dados são simulados e servem apenas para demonstração.

### Usar com dados reais

Para analisar uma turma real, substitua o conteúdo dos dois CSVs mantendo **os mesmos nomes de arquivo e as mesmas colunas** e execute o notebook novamente. Antes de divulgar qualquer resultado, confirme a autorização dos responsáveis e a anonimização dos dados (use apenas identificadores numéricos, sem nomes).

---

## Limitações

- **Dados simulados:** os resultados têm caráter ilustrativo e servem para validar a ferramenta, não para tirar conclusões sobre estudantes reais.
- **Amostra pequena (n = 40):** as análises são descritivas e exploratórias. Não são realizados testes de significância estatística.
- **Pouca variação de idade:** a amostra se concentra em 15 e 16 anos (34 dos 40 alunos), com apenas 3 alunos de 14 anos e 3 de 17 anos. A comparação por faixa etária deve ser lida com cautela.
- **Grupo único:** a distinção entre as duas turmas simuladas não é considerada.
- **Escala Likert:** por serem dados ordinais, a mediana e o IQR são complementares à média e ao desvio padrão na interpretação.

---

## Contexto acadêmico

- **Autor:** Anthony Hilário de Almeida e Oliveira
- **Instituição:** Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte (IFRN), Campus Currais Novos
- **Curso:** Tecnologia em Sistemas para Internet
- **Orientação:** *Italo Berg*
- **Ano:** 2026
