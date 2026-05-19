# Assinaturas Preditivas dos Tipos de Violência Contra a Mulher

Análise comparativa de algoritmos de aprendizado de máquina aplicada a 2,2 milhões de notificações de violência do SINAN (Sistema de Informação de Agravos de Notificação — Ministério da Saúde do Brasil).


## Sobre

O projeto investiga se os diferentes tipos de violência contra a mulher — física, psicológica e sexual — têm assinaturas preditivas distintas, ou se compartilham os mesmos padrões. A resposta tem implicações para políticas públicas: estratégias transversais de enfrentamento versus intervenções desenhadas por tipo de violência.

Para cada tipo de violência treinou-se um modelo binário independente, com avaliação em particionamento temporal (treino 2018–2023, teste 2024).

> Trabalho desenvolvido na disciplina de pós-graduação *Ciência de Dados e Aprendizado de Máquina* (doutorado, ICMC/USP — São Carlos) e adaptado para portfólio. Os notebooks priorizam código e resultados; a discussão metodológica detalhada está no artigo associado.

## Pipeline

| Notebook | Etapa |
|---|---|
| `01_ingestao` | Aquisição dos microdados do SINAN-VIOL via `pysus` |
| `02_auditoria_eda` | Auditoria das variáveis e análise exploratória |
| `03_pre_processamento` | Limpeza e tipagem |
| `04_analise_descritiva` | Estatística descritiva e análise espacial |
| `05_transformacao` | Auditoria de cardinalidade, agrupamento e pipeline de codificação |
| `06_modelagem` | Análise pré-modelagem, Regressão Logística, modelos não-lineares e avaliação |

## Abordagem

O pipeline parte do simples ao complexo. A Regressão Logística é o modelo de referência interpretável — coeficientes e odds ratios com intervalos de confiança. Modelos não-lineares (Random Forest, LightGBM) entram para testar se há estrutura de interação que o modelo linear não capta.

A avaliação não se limita à acurácia: em desfechos desbalanceados, prioriza-se recall, PR-AUC, balanced accuracy e calibração (Brier score). A seleção de variáveis é feita por regularização (L1, L2, Elastic Net), comparadas entre si.

## Principais resultados

- Cada tipo de violência apresenta uma assinatura preditiva própria — a violência física é marcada pelos meios de agressão; a psicológica, por ameaça e ausência de marcas físicas; a sexual, pela relação com o agressor e pela idade da vítima.
- O ganho dos modelos não-lineares sobre a Regressão Logística é pequeno nos três desfechos, indicando que o fenômeno é bem descrito por efeitos aditivos.
- O balanceamento de classes melhora o recall mas degrada a calibração das probabilidades — um trade-off relevante para qualquer uso prático.

## Como executar

Os notebooks foram desenvolvidos no Google Colab.

```bash
git clone https://github.com/RafaelaMlucca/violence-signatures-sinan.git
pip install -r requirements.txt
```

No início de cada notebook, ajuste a variável `PROJECT_PATH` para o diretório onde os dados intermediários serão armazenados. Execute os notebooks na ordem numérica.

Os microdados do SINAN são públicos e baixados automaticamente pelo notebook `01`. O módulo de acesso público não disponibiliza nomes ou identificadores individuais.

## Considerações éticas

Este é um trabalho de compreensão epidemiológica — identificar fatores associados a cada tipo de violência para informar políticas públicas. Não é um instrumento de triagem ou predição de risco individual. Os notebooks discutem os vieses dos dados de notificação (subnotificação, viés de preenchimento) e os limites de eventuais aplicações.

## Autora

**Rafaela Lucca** — Doutoranda, ICMC/USP

## Licença

MIT. Os microdados do SINAN são de domínio público (Ministério da Saúde do Brasil).
