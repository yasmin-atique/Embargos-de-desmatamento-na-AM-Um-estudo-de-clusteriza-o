![image](https://user-images.githubusercontent.com/101889306/203840956-42429811-c974-4f1c-a75e-3baffc4ec628.png)

#                  Modelo de Clusterização para identificação de Focos de Desmatamento na Floresta Amazônica e Análise Temporal
## Objetivos
- Localizar os núcleos de desmatamento na Floresta Amazônica;
- Realizar análise temporal por núcleo identificado.

## Fonte de dados
O dataset utilizado foi obtido do site do IBAMA em 07/11/2022.
https://servicos.ibama.gov.br/ctf/publico/areasembargadas/ConsultaPublicaAreasEmbargadas.php

## Metodologia
Foi utilizado o algoritmo não supervisionado HDBSCAN.
O HDBSCAN é um método hierarquico de clusterização, baseado em densidade. 

Diferentemente do DBSCAN, visto em aula, o HDB não possui o eps como parâmetro, uma vez que não utiliza um eps única, porém determina clusters diferentes com diferentes densidades.

Os principais parâmetros de entrada são:
- min_cluster_size - quanto maior o valor, menor a quantidade de clusters;
- min_samples - quanto maior o valor, maior a quantidade de pontos considerados como ruído.

Para este estudo, foram considerados como min_cluster_size = 10, e min_samples = 100.

## Tecnologia
- Python 3.9.7

## Serviços
- GitHub

## Bibliotecas python
- Pandas
- Numpy
- Regex
- Datetime
- Seaborn
- Pyplot
- Hdbscan

## Limpeza e validação dos dados
Após a importação dos dados foi possível notar que há uma grande quantidade de células NaN. Foram selecionadas as colunas para início da limpeza, conforme segue:

![image](https://user-images.githubusercontent.com/101889306/203857754-4e9a4284-68af-40fc-ae7a-a838ba9c41be.png)

1) Foi utilizado padrões regex para tratar NaN e validar a coluna 'Desmatamento';
2) A coluna 'UF_infração', o padrão regex [Aa][m][a][z] e o código do município foram utilizados para tratar os valores NaN da coluna 'Bioma';
3) Para o tratamento das colunas de coordenadas foram importados valores de outro dataset, que contem as coordenadas dos municípios brasileiros, segundo IBGE. Essa metodologia não apresentou resultados satisfatórios, portanto, não foi utilizada na limpeza do dataset final. Foram dropados valores NaN e valores que estavam incoerentes (indicam localização distante da Floresta Amazônica).
4) Após a realização das etapas de limpeza acima descritas, a coluna 'Data de inserção' não tinha mais nenhum valor NaN.

Após a conclusão de todas as etapas de limpeza de dados foram recuperadas 8.382 linhas do dataset original, correspondete a 3.192 (28%) do dataset final. Também foram validados 100% dos dados da coluna 'Desmatamento', os quais foram confirmados pelos padrões regex utilizados na coluna 'Infração'.

## Análise de dados

Como resultado do modelo, foi possível observar 26 focos de desmatamento ao longo da Floresta Amazônica, sendo a maior densidade de clusters situada ao sul, conforme pode ser obsevado na Figura a seguir.
![Figura 1 - Clusters](https://user-images.githubusercontent.com/101889306/204004944-60c2dc02-2918-4de3-94cd-1c70a7152813.jpg)

Foi feita análise temporal dos clusters encontrados, e foi possível notar um padrão de picos de desmatamento nos anos de 2008 e 2012, ou ambos os anos. Esses clusters foram agrupados e representados na Figura 2.

![Figura 2 - densidade por ano](https://user-images.githubusercontent.com/101889306/204004984-2c4c5c91-bb7d-41e2-b4dd-ccda5695700e.jpg)

Ao realizar avaliar a variação da quantidade de registros mensais por cluster, não foi possível notar padrões entre eles. Portanto, os clusters que apresentaram mais de 40 registros a cada bimestre foram reportados na Figura 3.

![Figura 3 - bimestre_REDUZ](https://user-images.githubusercontent.com/101889306/204005554-c4ed79cd-26d6-461f-9d1e-21997c745b07.jpg)
