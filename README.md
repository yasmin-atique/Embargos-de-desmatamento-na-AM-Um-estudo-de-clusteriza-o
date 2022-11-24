![image](https://user-images.githubusercontent.com/101889306/203840956-42429811-c974-4f1c-a75e-3baffc4ec628.png)

#                  Modelo de Clusterização para identificação de Focos de Desmatamento na Floresta Amazônica
## Objetivos
- Localizar os núcleos de desmatamento na Floresta Amazônica;
- Realizar análise temporal por núcleo identificado.

## Fonte de dados
O dataset utilizado foi obtido do site do IBAMA em 07/11/2022.
https://servicos.ibama.gov.br/ctf/publico/areasembargadas/ConsultaPublicaAreasEmbargadas.php

## Metodologia
Foi utilizado o algoritmo não supervisionado HDBSCAN.
O HDBSCAN é um método hierarquico de clusterização, baseado em densidade. Isso significa que ele funciona melhor com datasets com distribuição espacial complexa e grande densidade de dados.

Diferentemente do DBSCAN, visto em aula, o HDB não possui o eps como parâmetro. Faz parte do modelo testar vários eps até encontrar as características de entrada no modelo (ou default).

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
![image](https://user-images.githubusercontent.com/101889306/203857799-84bb4a25-4f8c-46a4-bb26-e9678132903b.png)

Foi feita análise temporal dos clusters encontrados, e foi possível notar um padrão de picos de desmatamento nos anos de 2008 e 2012, ou ambos os anos. Esses clusters foram agrupados e representados na Figura 2.

![image](https://user-images.githubusercontent.com/101889306/203858441-8d0d60d1-22a2-4cbc-a713-b99ab5f996a4.png)

Ao realizar avaliar a variação da quantidade de registros mensais por cluster, não foi possível notar padrões entre eles. Portanto, os clusters que apresentaram mais de 30 registros a cada bimestre foram reportados na Figura 3.

![image](https://user-images.githubusercontent.com/101889306/203859141-cb1ba5bf-d302-4b5e-9909-67e3bff50dca.png)

## Próximos passos
