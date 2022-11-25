![image](https://user-images.githubusercontent.com/101889306/203840956-42429811-c974-4f1c-a75e-3baffc4ec628.png)

#                  Modelo de Clusterização para Identificação de Focos de Desmatamento na Floresta Amazônica e Análise Temporal
## Objetivos
- Localizar os núcleos de desmatamento na Floresta Amazônica;
- Realizar análise temporal por núcleo identificado.

## Fonte de dados
O dataset utilizado foi obtido do site do IBAMA em 07/11/2022.
https://servicos.ibama.gov.br/ctf/publico/areasembargadas/ConsultaPublicaAreasEmbargadas.php

## Método
Foi utilizado o algoritmo não supervisionado HDBSCAN.
O HDBSCAN é um método hierarquico de clusterização, baseado em densidade. 

Diferentemente do DBSCAN, visto em aula, o HDB não possui o eps como parâmetro, uma vez que não utiliza um eps única, porém determina clusters diferentes com diferentes densidades.

Os principais parâmetros de entrada são:
- min_cluster_size - quanto maior o valor, menor a quantidade de clusters;
- min_samples - quanto maior o valor, maior a quantidade de pontos considerados como ruído.

Para este estudo, foram considerados como min_cluster_size = 10, e min_samples = 100.

## Tecnologia
- Python 3.9.7
- Surfer
- QGIS

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
Após a importação dos dados foi possível notar que há uma grande quantidade de valores NaN no dataset. Foram selecionadas as colunas para início da limpeza, as quais estão associadas com o objetivo deste trabalho.

A Tabela, a seguir, apresenta a quantidade de valores NaN no dataset, o tipo dos dados e as colunas selecionadas (destacadas em vermelho).

![image](https://user-images.githubusercontent.com/101889306/204061234-b8784aca-45e1-4215-bf26-18c068bd6055.png)

1) Foi utilizado regex para tratar NaN e validar a coluna 'Desmatamento';
2) A coluna 'UF_infração', o padrão regex [Aa][m][a][z] e o código do município foram utilizados para tratar os valores NaN da coluna 'Bioma';
3) Para o tratamento das colunas de coordenadas foram importados valores de outro dataset, que contem as coordenadas dos municípios brasileiros, segundo IBGE. Essa metodologia não apresentou resultados satisfatórios, portanto, não foi utilizada na limpeza do dataset final. Foram dropados valores NaN e valores que estavam incoerentes (indicam localização distante da Floresta Amazônica).
4) Após a realização das etapas de limpeza acima descritas, a coluna 'Data de inserção' não tinha mais nenhum valor NaN.

Após a conclusão de todas as etapas de limpeza de dados foram recuperadas 8.382 linhas do dataset original, correspondete a 3.192 (28%) do dataset final. Também foram validados 100% dos dados da coluna 'Desmatamento', os quais foram confirmados pelos padrões regex utilizados na coluna 'Infração'.

## Análise de dados

Como resultado do modelo, foi possível observar 26 focos de desmatamento ao longo da Floresta Amazônica, sendo a maior densidade de clusters situada ao norte do estado do Mato Grosso e por todo o estado de Rondônia e Pará, além das porções leste e oeste do Acre e leste do Roraima. Os centroides calculados não ignoram as ocorrências consideradas como ruído no modelo HDBSCAN, podendo indicar possíveis novas localizações de postos do Ibama.
A Figura 1, a seguir, apresenta a representação gráfica do modelo de clusterização.
![Figura 1 - clusters](https://user-images.githubusercontent.com/101889306/204063142-d036598a-cc47-4e63-bc87-381cc31929ce.jpeg)

Foi feita análise temporal dos clusters encontrados, e foi possível notar um padrão de picos de desmatamento nos anos de 2008 e 2012, ou ambos os anos. Esses clusters foram agrupados e representados na Figura 2.

![Figura 2 - densidade por ano](https://user-images.githubusercontent.com/101889306/204004984-2c4c5c91-bb7d-41e2-b4dd-ccda5695700e.jpg)

Ao avaliar a variação da quantidade de registros mensais por cluster, não foi possível notar padrões entre os membros dos grupos encontrados durante a avaliação anual. Portanto, os clusters que apresentaram mais de 40 registros a cada bimestre foram reportados na Figura 3, com o objetivo de orientar o Ibama na alocação de recursos.

![WhatsApp Image 2022-11-25 at 20 37 08](https://user-images.githubusercontent.com/101889306/204063330-d5fa34e8-26ff-4c95-a0bf-e354c9bb188a.jpeg)

## Próximos passos
- Avaliar onde estão os posto de atendimento do Ibama, para entender se os locais com menor quantidade de registros estão associados a falta de fiscalização.
