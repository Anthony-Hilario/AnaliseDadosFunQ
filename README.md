# Análise de Dados do FunQ: Jogo Educacional sobre Pensamento Computacional

Repositório com os dados e o notebook de análise utilizados no Trabalho de Conclusão de Curso **"AVALIAFUNQ: PLATAFORMA WEB PARA MENSURAÇÃO DA EXPERIÊNCIA LÚDICA DE ESTUDANTES COM SERIOUS GAMES"**, do curso de Tecnologia em Sistemas para Internet do IFRN (Campus Currais Novos).

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
| `Respostas_FunQ_Alunos.csv` | Respostas simuladas do FunQ (uma linha por aluno), no mesmo formato do arquivo exportado pela plataforma |
| `README.md` | Este arquivo |

### Sobre o CSV

- **`Respostas_FunQ_Alunos.csv`**: tem o mesmo formato do arquivo exportado pela plataforma (botão de exportação da área administrativa): campos separados por **ponto e vírgula** (`;`) e codificação UTF-8 com BOM.
- Colunas: `ID`, `Idade`, `Grupo de Teste`, `Pesquisador`, `q1` a `q18` (notas de 1 a 5) e `q19_opiniao_atividade` a `q24_sugestoes_mudanca` (respostas abertas, opcionais, que podem estar em branco).
- A idade fica na mesma tabela das respostas. Não há nomes nem qualquer outro dado pessoal. Se a idade estiver ausente no sistema, a plataforma exporta `N/A`, e o notebook trata esse valor como ausente.

---

## Como o FunQ é calculado

O FunQ possui 18 itens, respondidos em uma **escala de frequência de 1 a 5** (1 = Nunca, 5 = O tempo todo), distribuídos em 6 dimensões de 3 itens cada:

| Dimensão | Itens |
|---|---|
| Autonomia (A) | q1, q2, q3 |
| Desafio (C) | q4, q5, q6 |
| Prazer (D) | q7, q8, q9 |
| Imersão (I) | q10, q11, q12 |
| Redução de Barreiras Sociais (SB) | q13, q14, q15 |
| Estresse (S) | q16, q17, q18 |

**Itens invertidos [R]:** alguns itens têm pontuação inversa e são recodificados pelo sistema antes de serem gravados, pela fórmula `nota_invertida = 6 - nota` (1 vira 5, 2 vira 4, 3 permanece 3, 4 vira 2 e 5 vira 1). Os itens invertidos são **q2** (Autonomia) e **q16, q17 e q18** (Estresse). **A plataforma já grava esses valores invertidos**, por isso o CSV exportado já os traz recodificados e o notebook **não inverte novamente**.

**Pontuação de cada dimensão:** média simples dos três itens (já com a inversão aplicada), por aluno. Exemplo: `Desafio = (q4 + q5 + q6) / 3`. Como Estresse é composta apenas por itens invertidos, nela uma média **menor** indica **mais** estresse.

Instrumento original: TISZA, Gabriella; MARKOPOULOS, Panos. The Fun Questionnaire (FunQ): Measuring fun in learning with adolescents. *Computers & Education*, v. 195, p. 104712, 2023.

---

## O que o notebook faz

O notebook `analise-respostas-funq.ipynb` deve ser executado **na ordem das células**, pois as etapas posteriores dependem das variáveis criadas nas anteriores:

1. **Carregamento dos dados:** leitura de `Respostas_FunQ_Alunos.csv` (separador `;`).
2. **Cálculo das dimensões:** média dos três itens de cada dimensão, a partir dos valores já invertidos pelo sistema.
3. **Médias por dimensão:** gráfico de barras com o escore médio da turma.
4. **Estatística descritiva:** mediana e IQR, desvio padrão, mínimo, máximo e amplitude de cada dimensão.
5. **Correlação entre dimensões:** matriz de correlação (heatmap), incluindo relações como Desafio × Imersão e Autonomia × Prazer.
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

Mantenha o arquivo CSV na **mesma pasta** do notebook, pois ele o lê pelo nome (`Respostas_FunQ_Alunos.csv`).

---

## Guia para não-programadores

Esta seção é para professores e demais interessados que querem usar os arquivos sem experiência com programação. Há três caminhos, do mais simples ao mais completo.

### Caminho 1: apenas ver os dados (sem instalar nada)

Os arquivos `.csv` são tabelas simples e podem ser abertos no **Excel**, no **LibreOffice Calc** ou no **Google Planilhas** (Arquivo → Importar → Upload).

- `Respostas_FunQ_Alunos.csv`: cada linha é um aluno e cada coluna `q1` a `q18` é uma pergunta do questionário (notas de 1 a 5). Ao importar, escolha **ponto e vírgula** como separador.

Se quiser apenas conferir as médias por dimensão de cada aluno, a tela de administração da plataforma já mostra essa tabela pronta.

### Caminho 2: ver os gráficos e rodar a análise pelo navegador (recomendado)

Usa o **Google Colab**, que roda o notebook online, sem instalar nada. É necessária uma conta Google.

1. Acesse [colab.research.google.com](https://colab.research.google.com) e entre com sua conta Google.
2. Clique em **Arquivo → Fazer upload de notebook** e selecione `analise-respostas-funq.ipynb`.
3. No menu lateral esquerdo, clique no **ícone de pasta** e depois no ícone de **upload**. Envie o arquivo `Respostas_FunQ_Alunos.csv`.
4. Clique em **Ambiente de execução → Executar tudo**.
5. Role a página: os gráficos e tabelas aparecem abaixo de cada célula.

Observações:
- O CSV precisa ser enviado ao Colab **antes** de executar, porque o notebook os procura pelo nome.
- O Colab já traz `pandas`, `matplotlib` e `seaborn` instalados.
- Os arquivos enviados ao Colab somem quando a sessão termina. Basta repetir o passo 3 na próxima vez.

### Caminho 3: rodar no próprio computador

1. Instale o **Anaconda** ([anaconda.com/download](https://www.anaconda.com/download)), que já inclui Python, Jupyter e as bibliotecas necessárias.
2. Baixe este repositório (botão **Code → Download ZIP** no GitHub) e extraia a pasta.
3. Abra o **Anaconda Navigator**, inicie o **Jupyter Notebook** e navegue até a pasta extraída.
4. Abra `analise-respostas-funq.ipynb` e clique em **Run → Run All Cells** (ou execute célula por célula com `Shift + Enter`).

### Como ler os resultados

- Todas as notas vão de **1 a 5**.
- Depois da inversão dos itens [R], **valores mais altos indicam uma experiência mais favorável em todas as dimensões**, inclusive em Estresse, em que nota alta significa **menos** estresse.
- **Média e mediana** mostram o resultado típico da turma. **Desvio padrão, IQR e amplitude** mostram o quanto as respostas variam entre os alunos.
- Lembre-se: os dados são simulados e servem apenas para demonstração.

### Usar com dados reais

Para analisar uma turma real, substitua o arquivo pelo CSV exportado da plataforma, mantendo o **nome** `Respostas_FunQ_Alunos.csv` (é o nome que a plataforma já gera), e execute o notebook novamente. Antes de divulgar qualquer resultado, confirme a autorização dos responsáveis e a anonimização dos dados (use apenas identificadores numéricos, sem nomes).

---

## Limitações

- **Dados simulados:** os resultados têm caráter ilustrativo e servem para validar a ferramenta, não para tirar conclusões sobre estudantes reais.
- **Amostra pequena (n = 40):** as análises são descritivas e exploratórias. Não são realizados testes de significância estatística.
- **Pouca variação de idade:** a amostra se concentra em 15 e 16 anos (34 dos 40 alunos), com apenas 3 alunos de 14 anos e 3 de 17 anos. A comparação por faixa etária deve ser lida com cautela.
- **Grupo único:** a coluna `Grupo de Teste` (Grupo A e Grupo B) não é usada na análise, que considera os 40 alunos como uma única turma.
- **Escala Likert:** por serem dados ordinais, a mediana e o IQR são complementares à média e ao desvio padrão na interpretação.

---

## Contexto acadêmico

- **Autor:** Anthony Hilário de Almeida e Oliveira
- **Instituição:** Instituto Federal de Educação, Ciência e Tecnologia do Rio Grande do Norte (IFRN), Campus Currais Novos
- **Curso:** Tecnologia em Sistemas para Internet
- **Orientação:** *Italo Berg*
- **Ano:** 2026
