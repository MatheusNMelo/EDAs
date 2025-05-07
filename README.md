Esse Repositorio reune as EDAs desenvolvidas por mim em projetos ou estudos.

Abaixo alguns conhecimentos (colas), que são sempre bons em ter na mente




🔁 Testes para Comparar Médias
| Teste                     | Quando Usar                                                                       | Pré-requisitos                                                               |
| ------------------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **T-test (independente)** | Comparar médias de dois grupos diferentes (ex: média de compra mobile vs desktop) | Dados aproximadamente normais, variâncias semelhantes (ou usar Welch t-test) |
| **T-test pareado**        | Comparar duas medições do mesmo grupo (ex: antes e depois de uma campanha)        | Diferença entre pares deve ser aproximadamente normal                        |
| **ANOVA (one-way)**       | Comparar médias entre **3 ou mais grupos** (ex: gasto médio por região)           | Normalidade e homogeneidade das variâncias                                   |

from scipy.stats import ttest_ind
t_stat, p = ttest_ind(grupo1, grupo2, equal_var=True)  # use equal_var=False para Welch t-test

from scipy.stats import ttest_rel
t_stat, p = ttest_rel(pares_antes, pares_depois)

from scipy.stats import f_oneway
f_stat, p = f_oneway(grupo1, grupo2, grupo3)

📊 Testes para Associação entre Categóricas
| Teste                          | Quando Usar                                                                                     | Pré-requisitos                              |
| ------------------------------ | ----------------------------------------------------------------------------------------------- | ------------------------------------------- |
| **Qui-quadrado (Chi-squared)** | Testar associação entre duas variáveis categóricas (ex: tipo de dispositivo vs compra: sim/não) | Amostras grandes, frequências esperadas ≥ 5 |
| **Fisher's Exact Test**        | Mesma ideia do qui-quadrado, mas para tabelas pequenas (ex: < 5 observações por célula)         | Nenhum — usado quando amostra é pequena     |

from scipy.stats import chi2_contingency
tabela = [[20, 30], [15, 35]]  # Exemplo: [[compra_sim_mobile, compra_nao_mobile], ...]
chi2, p, dof, expected = chi2_contingency(tabela)

from scipy.stats import fisher_exact
tabela = [[8, 2], [1, 5]]
odds_ratio, p = fisher_exact(tabela)

📈 Testes de Normalidade
| Teste                       | Quando Usar                                                              | Comentários                            |
| --------------------------- | ------------------------------------------------------------------------ | -------------------------------------- |
| **Shapiro-Wilk**            | Verificar se os dados seguem uma distribuição normal                     | Bom para pequenos conjuntos de dados   |
| **Kolmogorov-Smirnov (KS)** | Comparar distribuição de dados com uma distribuição teórica (ex: normal) | Menos sensível que Shapiro             |
| **Anderson-Darling**        | Outra alternativa para testar normalidade                                | Mais sensível a caudas da distribuição |

from scipy.stats import shapiro
stat, p = shapiro(dados)

from scipy.stats import kstest
stat, p = kstest(dados, 'norm')  # Compara com distribuição normal

from scipy.stats import anderson
result = anderson(dados)
print(result.statistic, result.critical_values)

📐 Testes Não Paramétricos (quando os dados não são normais)
| Teste                                  | Quando Usar                                                 | Alternativa a  |
| -------------------------------------- | ----------------------------------------------------------- | -------------- |
| **Mann-Whitney U (Wilcoxon rank-sum)** | Comparar dois grupos independentes sem assumir normalidade  | T-test         |
| **Wilcoxon Signed-Rank**               | Comparar dois grupos dependentes (pareados) sem normalidade | T-test pareado |
| **Kruskal-Wallis**                     | Comparar mais de dois grupos sem normalidade                | ANOVA          |
| **Spearman**                           | Correlação entre variáveis ordinais ou não-lineares         | Pearson        |

from scipy.stats import mannwhitneyu
stat, p = mannwhitneyu(grupo1, grupo2, alternative='two-sided')

from scipy.stats import wilcoxon
stat, p = wilcoxon(pares_antes, pares_depois)

from scipy.stats import kruskal
stat, p = kruskal(grupo1, grupo2, grupo3)

📉 Testes de Correlação
| Teste           | Quando Usar                                                | Tipo de Dados                      |
| --------------- | ---------------------------------------------------------- | ---------------------------------- |
| **Pearson**     | Correlação linear entre duas variáveis contínuas e normais | Contínuas                          |
| **Spearman**    | Correlação entre variáveis ordinais ou não-lineares        | Ordinais ou contínuas não-lineares |
| **Kendall Tau** | Alternativa a Spearman, mais robusta com poucos dados      | Ordinais                           |

from scipy.stats import pearsonr
corr, p = pearsonr(x, y)

from scipy.stats import spearmanr
corr, p = spearmanr(x, y)

from scipy.stats import kendalltau
corr, p = kendalltau(x, y)