# Economia do açaí no Pará: preço ao produtor, dispersão regional e acesso

Código, dados processados e scripts de figuras da pesquisa sobre a formação e a
repartição espacial do preço ao produtor de açaí no Pará. O material sustenta uma
série de textos de divulgação e um projeto de pesquisa de mestrado.

Perguntas centrais: como se distribui o preço ao produtor entre os municípios
paraenses; se há convergência ou divergência ao longo do boom recente; e em que
medida o acesso (condição geográfica, isolamento fluvial) condiciona essa
repartição.

## Estrutura

```
dados/      bases processadas (ver proveniência abaixo)
R/          ingestão (SIDRA/IBGE) e pipeline de análise em R
python/     análise: dispersão, convergência, efeitos fixos, econometria espacial
figuras/    scripts que geram as figuras usadas nos textos
saidas/     destino das figuras geradas (conteúdo versionado é ignorado)
```

## Dados e proveniência

- `painel_acai_pa.csv` — painel municipal do açaí no Pará, 2015–2024 (quantidade,
  valor e preço implícito ao produtor). Fonte: IBGE, Produção Agrícola Municipal
  (PAM, tabela 5457), obtido via API de dados agregados do IBGE.
- `painel_acai_espacial.csv` — o painel acima acrescido de coordenadas e das
  variáveis de acesso (distância a Belém, insularidade). Gerado por
  `python/04_espacial_moran_sar_sem.py`.
- `painel_municipal_real.csv` — recorte municipal 2021–2022 (9 municípios).
  Fonte: SEDAP-PA e CEEAC/FAPESPA, sobre PAM/IBGE.
- `icn_fapespa.csv` — Índice de Concentração Normalizado, 2015 e 2022.
  Fonte: CEEAC/FAPESPA, sobre PAM/IBGE.
- `acai_pam_real.csv` — produção e valor do açaí por estado (Pará, Amazonas,
  Brasil), 2019–2023. Fonte: IBGE/PAM (via FAPESPA).
- `municipios_br.csv` — centroides municipais por código IBGE (latitude e
  longitude). Fonte: base pública de municípios brasileiros (kelvins/municipios-brasileiros).

Ressalva metodológica importante: o preço municipal é implícito (valor dividido
pela quantidade) e, em parte, reflete preços de referência atribuídos pelo IBGE em
nível regional. As análises privilegiam padrões regionais, plurianuais e
ponderados pela produção, e evitam leitura fina de ranking entre municípios.

## Como executar

Python (a partir da raiz do repositório):

```bash
pip install -r requirements.txt
python python/01_dispersao_convergencia.py
python python/04_espacial_moran_sar_sem.py   # gera dados/painel_acai_espacial.csv
python figuras/fig_cap3_mapa.py               # figuras vão para saidas/
```

R:

```r
install.packages(c("jsonlite","dplyr","ggplot2","fixest"))
# ingestão do painel completo (requer acesso ao IBGE):
Rscript R/01_ingestao_sidra.R 5457 15 1994 2024 dados/painel_acai_pa.csv
# análise:
Rscript R/02_analise_painel.R dados/painel_acai_pa.csv
```

## Proveniência: script, saída e texto correspondente

| Script | Produz | Texto |
|---|---|---|
| `R/01_ingestao_sidra.R` | painel municipal (SIDRA) | base de toda a série |
| `R/02_analise_painel.R` | dispersão, convergência, efeitos fixos (R) | pipeline de trabalho |
| `python/01_dispersao_convergencia.py` | CV, Theil, convergência σ e β, efeitos fixos | Capítulo 2 |
| `python/02_analise_municipal_regional.py` | rankings, casos municipais, comparação regional | Capítulo 2 / discussão |
| `python/03_decomposicao_regional_convergencia.py` | razão regional no tempo, janelas de convergência | Capítulo 2 (revisão) |
| `python/04_espacial_moran_sar_sem.py` | acesso, pesos espaciais, I de Moran, SAR/SEM | Capítulo 3 |
| `python/05_robustez_temporal.py` | estabilidade temporal, efeitos totais, robustez a W | Capítulo 3 (científico) |
| `figuras/fig_cap1_decomposicao_estado.py` | decomposição valor/preço/quantidade | Capítulo 1 |
| `figuras/fig_cap1_dispersao_icn.py` | dispersão municipal (2022) e ICN | Capítulo 1 |
| `figuras/fig_cap2_regiao_escala.py` | preço por região; volume × preço | Capítulo 2 |
| `figuras/fig_cap2_escala_preco_final.py` | volume × preço (rótulos ajustados) | Capítulo 2 |
| `figuras/fig_cap3_mapa.py` | distribuição geográfica do preço | Capítulo 3 |
| `figuras/fig_cap3_coef_tempo.py` | coeficiente de insularidade por ano | Capítulo 3 |

## O que não está incluído (e por quê)

Este repositório reúne apenas o que foi estritamente validado e sustentou texto
publicado. Ficaram de fora, de forma deliberada: os protocolos de transmissão
vertical e de pass-through (NARDL), que permaneceram em estágio exploratório e
rodaram sobre dados sintéticos; os geradores de dados sintéticos e de figuras
ilustrativas; a ingestão via pacote `sidrar`, substituída pela versão por API; o
esboço de ingestão de dados de campo, não utilizado; e os scripts de formatação de
documentos e banners, que são de apresentação, não de pesquisa.

## Uso de inteligência artificial

O código de análise e os scripts de figuras deste repositório foram desenvolvidos
com assistência de ferramenta de inteligência artificial generativa, sob direção,
revisão e validação do autor. Os dados, os resultados e sua interpretação são de
responsabilidade do autor.

## Licença

Código sob licença MIT (ver `LICENSE`). Os dados derivam de fontes públicas (IBGE,
FAPESPA, SEDAP) e permanecem sujeitos aos termos dessas fontes.
