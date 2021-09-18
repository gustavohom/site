---
title: "Padronizando Estilos"
date: '2021-09-18'
description: Padrão de escrita de scripts, tabelas, arquivos e pastas
output:
  html_document:
    df_print: paged
hero: snakes_vs_camels.png
menu:
  sidebar:
    name: Padronizando estilos
    identifier: pradradonizando-estilos
    parent: linguagem-r
    weight: 40
---

Essa é uma tentativa de padronizar as nomeclatoras de variaveis, objetos, classes, arquivos, colunas e linhas de tabelas.
---

# Padronização

## Introdução

A forma como lemos e entendemos o mundo ao nosso redor é derivado de padrões. Ao vermos sistematicamente caracteristicas conceituais de uma determinada forma, aprendemos e associamos estas caracteristicas como uma representação generalizada deste item. Como exemplo: você provavelmente consegue identificar um palhaço apenas olhanndo para ele 🤡. As forma de se vestir, os adornos e a maquiagem permite que você separe alguém vestido de palhaço de um empresário qualquer.

Na escrita, esse fato também acontece. O simples fato de você ler este texto demonstra o quão importante é manter os padrões. Se os simbolos  de uma escrita variassem em relação a cada escritor, jamais seriamos capaz de transmitir conhecimento. 

Nas linguagens computacionais este fato é uma meia verdade. Apesar de as regras de escrita de que cada linguagem mantenha padrões obrigatórios, os programadores são livres para definir a nomeclatura de variaveis, objetos, arquivos, etc. Para melhor visualizarmos esse fato, imaginemos que um progrador queira salvar um vetor contendo nome de três alunas de graduação em uma variavel qualquer. Então ele pode salvar algo como:

```text
nA

Alunasghs

variavelDEnomes

ALUNAS_nomes

NomAluNEsa

Variavel1

X

nomealunas

nomeAlunas

NomeAlunas

nome.alunas

nome_alunas

NOMEALUNAS
```
 
 
Percebemos então que apesar de ser uma variavel contendo três nomes, a nomeclatura para ela pode variar de acordo com cada autor. A tentativa deste capitulo não é ter um padrão obrigatório quanto a escrita e quais nomes devem ser usados, mas um guia para auxiliar a escrita mais eficaz dos nomes.

## Princiapais estilos

Não me aprofundarei na história ou a motivação de cada um dos estilos que citarei a seguir. Alguns dos principais estilos de escrita na programação e seus respectivos exemplos:
 

| Estilo | Exemplo |
| :--- | :--- |
| Lower Case | nomealunas |
| Upper Case | NOMEALUNAS |
| Mixed Case | nOmEaLuNa |
| Lower Upper | nomeALUNA |
| Upper Lower | NOMEaluna |
| Camel Case | nomeAlunas |
| Pascal Case | NomeAlunas |
| Snake Case \(under\) | nome\_aluna |
| Snake Case \(upper\) | NOME\_ALUNA |
| Kebab Case | nome-aluna |
| Dot Case | nome.aluna |

 
## Geral

Não deve-se usar:

* Pontos
* Caracteres especiais
* Espaço
* Simbolos
* Acentos
* Palavras fora de contexto
* Não incluir conectores como `de`, `da`, `dos`, `e`, `a`, `em`, etc.

Pode-se usar

* Letras
* Palavras
* Numeros
* Os sibolos **`-`**   e   **`_`**

## **Pastas**

Um estilo de padronização de pastas para dois projetos que usam o mesmo banco de dados. A pasta input são dos dados corrigidos , filtrados ou separados do banco de dados original. Os arquivos output são as saidas finais para os arquivos. Estou tentando criar uma pasta chamada `processados`para os processos intermediarios, mas sem eficacia para saida final, como por exemplo: Na pasta dados tenho um raster. Em input tenho um recorte do raster especificando a área de interesse. Em `processados`tenho o filtro para retirada de outliers do raster. Em output os arquivos processados do raster. Em entrega os produtos de interesse finalizados. Essa organização permite com que os dados sejam consultados pelo caminho relativo `../../dados` . A pasta `src` contém scripts que iremos chamar com `source()`, como funções por exemplo.

Os dados brutos devem ser apenas para leitura. 
 
 
```text
PROJETOS COM MESMO BANCO DE DADOS

Pasta raiz
+-- local-01
|    +-- dados
|    |   +-- imagem
|    |   +-- tabela
|    |   +-- video
|    |   +-- vetor
|    \-- projetos
|        +-- inventario_florestal
|         |    +-- 00_scripts
|         |    |   +-- src
|         |    +-- 01_input
|         |    |   +-- tb
|         |    |   +-- geo
|         |    |   +-- obj
|         |    +-- processados
|         |    +-- 02_output
|         |    |   +-- grafico
|         |    |   +-- tabela
|         |    |   +-- geo
|         |    \-- 03_entrega
|         \-- cubagem_arvores
\-- local-02


PROJETOS COM DIFERENTES BANCOS DADOS

+-- local-01
    +-- scripts
    +-- output
    +-- dados
    |   +-- brutos
    |   +-- processados
    +-- docs
    \-- entrega
        +-- relatorio
```
 
  
As pastas podem ser escritas seguindo as regras gerais. Preferencialemente devem ser escritas em sneak\_case com letras minúsculas. Recomendo usar underline \( \_ \) para criação de contexto e traço \( - \) para sequenciamento, referencias geografica, data, individualizações ... Exemplo:
 

* `projeto_carbono-01`
* `projeto_carbono-02`
* `mapas-mg`
* `mapas-rj`
* `mapas_minas`_`_`_`gerais-bh`
* `faturamento-2021-08-13`
 

Sempre que possível, adicione uma breve descrição do projeto em um arquivo README.txt ou README.md

## Arquivos

Os arquivos seguem a mesma terminologias que as pastas, porém com algumas modificações:

* Os gráficos podem ter contexto com definido o tipo de grafico e dados usados
  * `col_carro_consumo` : gráfico de coluna com eixos carro e consumo
  * `dis_aluna_nota` : gráfico de disperção de com eixos aluna e nota
  * `dis_mes_nota-Gabriely` : gráfico de dispersão de nota ao longo dos meses da aluna Grabriely
* De preferência para palavras no singular
* Prefixos especificam o tipo de arquivo e sufixos os processos
  * `dtm_solos_declivosos`
  * `dtm_solos_declivosos-itamarandiba`
  * `dtm_solos_declivosos-itamarandiba_filtrado`
* Prefixos também podem ser invertidos com sufixos para agregamento de dados
  * `2021-02-corte_eucalipto`
  * `2021-02-corte_pinus`
  * `2019-03-corte_pinus`
  * `2019-03-corte_teca`
* É aconselhável o uso de sufixo para arquivos que seguem uma sequencia lógica
  * `01_filtrar_dados`
  * `02_calcular_media`
  * `03_grafico_medias`

## Tabelas

Essa parte achei interessante um artigo da **Emily Riederer.** Estou adotando o padrão definido por ela como referência para meus projetos. O link para o artigo original: [https://emilyriederer.netlify.app/post/column-name-contracts/](https://emilyriederer.netlify.app/post/column-name-contracts/)

Abaxo segue um copiar e colar traduzido do blog dela

> ### Vocabulário Controlado <a id="controlled-vocabulary"></a>
>
> A ideia básica de vocabulários controlados é definir antecipadamente um conjunto de palavras, frases ou esboços com significados bem definidos que podem ser usados ​​para indexar informações. Quando esses stubs são definidos para diferentes tipos de informações e peças juntas em uma ordem consistente, o vocabulário se torna uma gramática descritiva que podemos usar para descrever um conteúdo e comportamento mais complexos.
>
> No contexto de um conjunto de dados, esse vocabulário também pode servir como um contrato latente entre produtores e consumidores de dados e conter promessas relacionadas a diferentes aspectos da linhagem de dados, valores válidos e usos apropriados. Quando usado de forma consistente em todas as tabelas de uma organização, pode escalar significativamente o gerenciamento de dados e aumentar a usabilidade, pois o conhecimento do trabalho com um conjunto de dados é facilmente transferido para outro.
>
> Por exemplo, imagine que trabalhamos em uma empresa de compartilhamento de viagens e estamos construindo uma tabela de dados com um registro por viagem. Qual pode ser a aparência de um vocabulário controlado? [1](https://emilyriederer.netlify.app/post/column-name-contracts/#fn:1)
>
> **Nível 1: Tipos de Medidas**
>
> Por razões que ficarão evidentes nos exemplos, gosto que o primeiro nível da hierarquia geralmente capture um “tipo” semi-genérico da variável. Esta não é bem é o mesmo que tipos de dados em uma linguagem de programação \(por exemplo  `bool`, `double`, `float`\), apesar de tudo com o mesmo prefixo deve finalmente ser escalado para o mesmo tipo. Em vez disso, esses tipos de dados implicam um tipo de informação e padrões de uso apropriados:
>
> * `ID`: Único identificado para uma entidade.
>   * Numérico para armazenamento e junções mais eficientes, a menos que o sistema de registro gere IDs com caracteres
>   * Provavelmente uma chave primária em alguma outra tabela
> * `IND`: Indicador binário 0 ou 1 ou uma ocorrência de evento
>   * Porque sempre 0 ou 1, pode ser calculada a média para encontrar a ocorrência de proporção
>   * Pode considerar chamar em `IS`vez de `IND`para ainda menos ambiguidade, caso que é rotulado como 1
> * `N`: Contagem de quantidade ou ocorrência de evento
>   * Sempre um número inteiro não negativo
> * `AMT`: Montante em número real que pode ser somado. Ou seja, qualquer valor não contável que seja "livre de denominador"
> * `VAL`: Variáveis ​​numéricas que não são inerentemente somadas
>   * Por exemplo, taxas e proporções que não podem ser combinadas ou valores numéricos como latitude e longitude para os quais as operações aritméticas típicas não fazem sentido
> * `DT`: Data de algum evento
>   * Sempre elenco como um `YYYY-MM-DD`encontro
> * `TM`: Carimbo de data / hora de algum evento
>   * Sempre elenco como um `YYYY-MM-DD HH:MM:SS`carimbo de data / hora
>   * Distinguir datas de carimbos de hora evitará junções defeituosas de dois campos de data organizados de forma diferente
> * `CAT`: Variável categórica como uma sequência de caracteres \(potencialmente codificada de um `ID`campo\)
>
> Embora sejam relativamente genéricos, categorias específicas de domínio também podem ser usadas. Por exemplo, como a localização é tão importante para o compartilhamento de carona, pode valer a pena tê-la `ADDR`como uma categoria de nível 1.
>
> **Nível 2: Medir Assuntos**
>
> A melhor hierarquia varia amplamente de acordo com o setor e o _conteúdo geral de um banco de dados_ - não apenas uma tabela. Aqui, esperamos estar interessados ​​em atribuições de viagem sobre muitos assuntos diferentes: o piloto, o motorista, a viagem, etc., portanto, o assunto da medida pode ser o próximo nível lógico. Podemos definir:
>
> * `DRIVER`: Informações sobre o motorista
> * `RIDER`: Informações sobre o passageiro, o passageiro que ligou para o compartilhamento de viagem
> * `TRIP`: Informações sobre a própria viagem
> * `ORIG`: Informações sobre o início da viagem \(horário e geografia\)
> * `DEST`: Informações sobre o destino da viagem \(horário e geografia\)
> * `COST`: Informações sobre os componentes do custo total \(pode ser um subconjunto de `TRIP`, mas pertence a todas as partes e tem alta cardinalidade na próxima camada, então vamos dividi-la\)
>
> Obviamente, em um banco de dados altamente normalizado, as medidas dessas diferentes entidades existiriam em tabelas separadas. No entanto, essa disciplina em nomeá-los ainda seria benéfica, de modo que as quantidades não são ambíguas quando um analista as combina.
>
> **Níveis 3-n: Detalhes**
>
> As primeiras camadas da hierarquia são críticas para padronizar para fazer nossas “promessas de desempenho” e ajudar na capacidade de pesquisa de dados. Os níveis posteriores terão medidas específicas e podem não valer a pena defini-los antecipadamente. No entanto, para conceitos que irão existir em muitas tabelas, vale a pena pré-especificar seus nomes e formatos precisos. Por exemplo:
>
> * `CITY`: Deve ser em maiúsculas? Como os espaços no nome da cidade devem ser tratados?
> * `ZIP`: Devem ser usados ​​CEPs de 6 ou 10 dígitos?
> * `LAT`/ `LON`: Para quantas casas decimais a latitude e a longitude devem ser geocodificadas? Se a empresa opera apenas em certas áreas geográficas \(por exemplo, os EUA continental\), cortes grosseiros para estes podem ser determinados
> * `DIST`: A distância é medida em milhas? Quilômetros?
> * `TIME`: As durações são medidas em segundos? Minutos?
> * `RATING`: Quais são os intervalos válidos para outras quantidades conhecidas, como classificações por estrelas?
>
> Os “adjetivos” terminais também podem ser considerados. Por exemplo, se os sistemas de geração de dados cuspirem quantidades analiticamente unideais que devem ser preservadas para fins de linhagem de dados, sufixos como `_RAW`e `_CLEAN`podem denotar a versão da mesma variável em seus estados original e bem cuidado, respectivamente.
>
> **Juntando tudo**
>
> Essa estrutura agora nos dá uma gramática para nomear compactamente 35 variáveis ​​na tabela:
>
> * `ID_{DRIVER | RIDER | TRIP}`: Identificador único para a festa da viagem
> * `DT_{ORIG | DEST}`: Data de início e término da viagem, respectivamente
> * `TM_{ORIG | DEST}`: Carimbo de data / hora no início e no fim da viagem, respectivamente
> * `N_TRIP_{PASSENGERS | ORIG | DEST}` Contagem de passageiros únicos, pontos de embarque e desembarque da viagem
> * `N_DRIVER_SEATS`: Número de assentos de passageiros disponíveis no carro do motorista
> * `AMT_TRIP_{DIST | TIME}`: Distância total da viagem em milhas percorridas e tempo gasto
> * `AMT_COST_{TIME | DIST | BASE | FEES | SURGE | TIPS}`: Quantidade de cada componente de custo
> * `IND_SURGE`: Variável indicadora se a viagem for calculada durante o aumento do preço
> * `CAT_TRIP_TYPE`: Tipo de viagem, como 'Pool', 'Standard', 'Elite'
> * `CAT_RIDER_TYPE`: Status do passageiro, como 'Básico', 'Freqüente', 'Assinatura'
> * `VAL_{DRIVER | RIDER}_RATING`: Classificação média por estrelas do piloto e do motorista
> * `ADDR_{ORIG | DEST}_{STREET | CITY | STATE | ZIP}`: Endereço de componentes de início e fim da viagem
> * `VAL_{ORIG | DEST}_{LAT | LON}`: Latitude e longitude de início e fim da viagem
>
> O fato de podermos descrever 35 variáveis ​​em aproximadamente 1/3 do número de linhas já mostra o valor dessa estrutura em ajudar os consumidores de dados a construir um modelo mental forte para manipular rapidamente os dados. Mas agora podemos demonstrar um valor muito maior.
>
> Para começar, criamos um pequeno conjunto de dados falso usando nosso esquema. Para simplificar, simulo 18 das 35 variáveis ​​listadas acima:
 
 

```text
head(data_trips)

#>   ID_DRIVER ID_RIDER ID_TRIP    DT_ORIG    DT_DEST N_DRIVER_PASSENGERS
#> 1      5698     1027    9714 2019-12-28 2019-12-28                   1
#> 2      6458     7822    7405 2019-09-07 2019-09-07                   1
#> 3      4838     5968    8554 2019-05-14 2019-05-14                   2
#> 4      3785     2309    1241 2019-08-16 2019-08-16                   2
#> 5      4539     9109    2017 2019-03-25 2019-03-25                   1
#> 6      4007     6878    1698 2019-09-09 2019-09-09                   1
#>   N_TRIP_ORIG N_TRIP_DEST AMT_TRIP_DIST IND_SURGE VAL_DRIVER_RATING
#> 1           1           1      24.98156         0          2.228128
#> 2           1           1      11.32751         1          3.678452
#> 3           1           1      25.71858         1          1.531821
#> 4           1           1      13.86827         0          1.982823
#> 5           1           1      25.05063         1          2.371799
#> 6           1           1      16.00619         0          1.585752
#>   VAL_RIDER_RATING VAL_ORIG_LAT VAL_DEST_LAT VAL_ORIG_LON VAL_DEST_LON
#> 1         3.253223     41.24602     40.48390    108.44754    102.87971
#> 2         3.466021     40.26759     41.66849    108.23701     93.70541
#> 3         3.316094     40.40432     40.94727     82.81049     90.85842
#> 4         4.862184     40.52839     41.43372     71.78928    119.20213
#> 5         4.436828     40.29525     41.86572    112.69544    118.53023
#> 6         1.217744     41.29352     41.79860    117.98442    108.41519
#>   CAT_TRIP_TYPE CAT_RIDER_TYPE
#> 1         Elite       Frequent
#> 2         Elite   Subscription
#> 3          Pool          Basic
#> 4          Pool          Basic
#> 5         Elite       Frequent
#> 6      Standard          Basic
```
 
  

## No R

### Algumas boas práticas

* Começe o codigo sempre com o nome do autor, data de inicio do projeto, nome do projeto, uma pequena descrição do projeto.
* Particione seu codigo usando `ctrl` + `shift` + `r` ou `#----` ou `#====`  exemplo:

```r
# Parte 1 --------------------------

a <- c("teste", "teste2")

# Parte 2 --------------------------

print(a)
```
 
  
* Comente os códigos e nomeie os chunks!!!
* Use `library( )` e não `require( )`
* Não use `atach( )` e tente evitar `setwd( )`
* Não salve `workspace`
* As variaveis devem ter preferencialmente o nome no singular. Quando obrigatoriamente escrito no plural, priorize o uso de coletivo. Exemplo: `funcionarios`deve ser trocado por `equipe`.
* Carregue os pacotes no inicio do código
* Em seguida, carregue os dados
* Depois, carregue as funções \(source\)
* Por fim, escreva os demais itens do código
* Prefira não usar o `return` implicito do R, escreva `return(x)`.
* Atibua variaveis e funções com `<-` e não `=`
* Variaveis escopo local com `_` no final, por exemplo `carro_`, isso evitará que funções procurem variaveis fora do escopo planejado.
* Nomes das variaveis devem ser ou começar o contexto como **substantivos** e as ****funções como **verbos**.

### Estilo dentro do R

| Tipo | Estilo | Exemplo |
| :--- | :--- | :--- |
| Variaveis | snake\_case | nome\_alunas |
| Funções | snake\_case | calcular\_aprovacao |
| Classes | snake + Pascal case | Projeto\_Casa |
| Objetos criados por classes | snake + camel case | casa\_Construida |
| Metodos | snake\_case | dimencionar\_casa |
| Interface | lower ou  snake\_case | txt ou txt\_descricao |
 
 
Para as variaveis podemos escrever um prefixo definindo o tipo de arquivo
 
 
| Abreviação | Tipo |
| :--- | :--- |
| vec\_ | vetor |
| con\_ | constante |
| tab\_ | tabela |
| lst\_ | lista |
| mat\_ | matriz |
| img\_ | imagem |
| pnt\_ | ponto |
| pol\_ | poligono |
| rst\_ | raster |
| bin\_ | binario |
 
 
Os sufixo podem especificar os processos posteriores, com uso de `_` ou `.`

1. vec\_consumo\_gasolina
2. vec\_consumo\_gasolina\_dez\_anos
3. vec\_consumo\_gasolina.dez\_anos

## Referências

https://emilyriederer.netlify.app/post/column-name-contracts/

imagem: https://codebrahma.com/intercepting-the-case-battle-between-snakes-and-camels-in-front-end-javascript/

https://www.youtube.com/watch?v=wXIuvdhMOsE&list=TLPQMTMwOTIwMjGPNNiIEa2kEA&index=9&gt

https://style.tidyverse.org/index.html

https://nicercode.github.io/

https://chrisvoncsefalvay.com/2018/08/09/structuring-r-projects/

https://kdestasio.github.io/post/r\_best\_practices/

https://basedosdados.github.io/mais/style\_data/\#nomea%C3%A7%C3%A3o-de-bases-e-tabelas
