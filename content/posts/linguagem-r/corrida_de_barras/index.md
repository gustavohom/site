---
title: "Corrida de Barras de Florestas Plantadas"
date: '2021-10-15'
description: Criando corrida de barras com R
output:
  html_document:
    df_print: paged
hero: floresta.jpg
menu:
  sidebar:
    name: Corrida de Barras
    identifier: corrida-de-barras
    parent: linguagem-r
    weight: 40
---

Corrida de Barras é uma forma divertida de apresentar resultados aos clientes e em apresentações. Nela é possível ver como os dados se alteram ao longo de um ciclo. Usarei a Linguagem R para demonstrar como criar este tipo de gráfico. Veja um exemplo abaixo referente a área de Florestas Plantadas no Brasil por Estado entre os anos de 2006 e 2019:

{{< img src="bar.gif" align="center" title="Florestas Plantadas no Brasil">}}

## Dados

Os dados são referentes aos plantios comerciais de florestas nos estados brasileiros, entre os anos de 2006 e 2019.
Foram selecionados apenas os plantios de Pinus e Eucalipto. Os dados que contemplam os anos de 2006 a 2016  foram coletados pela Indústria Brasileira de Árvores (Ibá)  e fornecidos  pelo site do [Sistema Nacional de Informações Florestais (SNIF)](https://snif.florestal.gov.br/pt-br/florestas-plantadas). Os valores dos anos de 2017 a 2019 foram obtidos no [Relatório aual 2020 do Ibá](https://iba.org/datafiles/publicacoes/relatorios/relatorio-iba-2020.pdf).

Vamos olhar as primeiras linhas da tabela:

Ano|Cultura|Espécie|Estado|Área (ha)
| :--: | :--: | :--: | :--: | :--: |
|31/12/2006|Eucalipto|Eucalipto|Minas Gerais|1181429
|31/12/2006|Eucalipto|Eucalipto|São Paulo|915841
|31/12/2006|Eucalipto|Eucalipto|Mato Grosso do Sul|119319
|31/12/2006|Eucalipto|Eucalipto|Bahia|540172
|31/12/2006|Eucalipto|Eucalipto|Rio Grande do Sul|184245
|31/12/2006|Eucalipto|Eucalipto|Espírito Santo|207800
|31/12/2006|Eucalipto|Eucalipto|Maranhão|93285
|31/12/2006|Eucalipto|Eucalipto|Paraná|121908
|31/12/2006|Eucalipto|Eucalipto|Mato Grosso|113770
|31/12/2006|Eucalipto|Eucalipto|Pará|115806
|31/12/2006|Eucalipto|Eucalipto|Goiás|98765
|31/12/2006|Eucalipto|Eucalipto|Tocantins|13901

</br>

Você pode baixar a planilha completa [neste link](https://github.com/gustavohom/site/blob/source/content/posts/linguagem-r/corrida_de_barras/dados.csv).


## Código R

O código R foi inspirado em um script criado por [Raja (2019)](https://towardsdatascience.com/create-animated-bar-charts-using-r-31d09e5841da). O script criado para este artigo pode ser consultado [clicando neste link](https://github.com/gustavohom/exemplos/tree/main/Corrida_de_barras).

## Pacotes

Foram usados os pacotes {<span style="color:orange">ggplot2</span>}, {<span style="color:orange">gganimate</span>}, {<span style="color:orange">tidyverse</span>}, {<span style="color:orange">magrittr</span>} e {<span style="color:orange">lubridate</span>}.

Para instala-los e carrega-los, usei a funcção **`m_load`** do pacote {<span style="color:orange">gmourao</span>}. Esta função instala, atualiza e carrega todos os pacotes necessários automaticamente.

```{r}

# Instalando e carregando pacotes -----------------------------------------

# remotes::install_github("gustavohom/gmourao")

pkg <- c("ggplot2", "gganimate", "tidyverse", "magrittr", "lubridate")

gmourao::m_load(pkg)

```
## Pré-processamento de dados

Primeiro inicie carregando os dados. Nesta parte do código: é criado uma coluna referente ao ano das informações; são retirados os valores **`Não informados`** e **`NA`**; são resumidas as áreas de florestas plantadas de eucalipto e pinus por **`Estado`** e **`Ano`**; são ranqueados os estados e apenas os dez primeiros colocados em cada ano permanecerão no gráfico.

```{r}

# Carregando dados --------------------------------------------------------

dados <-read.csv(file = "c://dados.csv", 
                                   header = TRUE, sep = ";", dec = ".")

# Corrigindo e resumindo os dados -----------------------------------------


dados$Ano %<>% dmy %>% year(.)

dados %<>% drop_na (.) %>%
  as.tibble(.) %>% 
  filter(Estado != "Não informado") %>% 
  group_by(Estado, Ano)%>% 
  summarise(area = sum(Área..ha.))


# Ranquendo os dados ------------------------------------------------------


dados %<>%
  group_by(Ano) %>%
  mutate(rank = rank(-area),
         area_rel = area/area[rank==1],
         area_lbl = paste0(" ",area)) %>%
  group_by(Estado) %>%
  filter(rank <=10)%>%
  ungroup()
  
```
</br>

## Construindo plotagens estáticas

Agora serão construidos todos as plotagens estáticas:

```{r}

# Criando plot ------------------------------------------------------------


staticplot = ggplot(dados, aes(rank, group = Estado,
                                       fill = as.factor(Estado), color = as.factor(Estado))) +
  geom_tile(aes(y = area/2,
                height = area,
                width = 0.9), alpha = 0.8, color = NA) +
  geom_text(aes(y = 0, label = paste(Estado, " ")), vjust = 0.2, hjust = 1, size = 10) +
  geom_text(aes(y=area,label = area_lbl, hjust=0), size = 10) +
  coord_flip(clip = "off", expand = FALSE) +
  scale_y_continuous(labels = scales::comma) +
  scale_x_reverse() +
  guides(color = FALSE, fill = FALSE) +
  theme(axis.line=element_blank(),
        axis.text.x=element_blank(),
        axis.text.y=element_blank(),
        axis.ticks=element_blank(),
        axis.title.x=element_blank(),
        axis.title.y=element_blank(),
        legend.position="none",
        panel.background=element_blank(),
        panel.border=element_blank(),
        panel.grid.major=element_blank(),
        panel.grid.minor=element_blank(),
        panel.grid.major.x = element_line( size=.1, color="grey" ),
        panel.grid.minor.x = element_line( size=.1, color="grey" ),
        plot.title=element_text(size=35, hjust=0.5, face="bold", colour="black", vjust=-1),
        plot.subtitle=element_text(size=22, hjust=0.5, face="italic", color="black"),
        plot.caption =element_text(size=22, hjust=0.5, face="italic", color="grey"),
        plot.background=element_blank(),
        plot.margin = margin(2,5, 2, 10, "cm")
        )
```
</br>

## Animação

Esta parte criará a animação

```{r}

# Criando animação --------------------------------------------------------


anim = staticplot + transition_states(Ano, transition_length = 4, state_length = 1) +
  view_follow(fixed_x = TRUE)  +
  labs(title = 'Florestas Plantadas no Brasil no ano de: {closest_state}',
       subtitle  =  "Eucalipto e Pinus (Valor em hectares)",
       caption  = "Feito por Gustavo Mourão (https://mourao.netlify.app) | Fonte dos Dados: Ibá (2017, 2018, 2020)")


```
</br>

## Renderização

Por fim será renderizada a animação em GIF e MP4.

#### GIF

```{r}
animate(anim, 200, fps = 30,  width = 2000, height = 1000,
        renderer = gifski_renderer("gganerwqim.gif"))
```
</br>

#### MP4 

```{r}

animate(anim, 200, fps = 20,  width = 2000, height = 1000,
        renderer = ffmpeg_renderer()) -> for_mp4anim_save("animation.mp4", animation = for_mp4 )

```
</br>

Espero que tenha gostado. Que esse pequeno tutorial te sirva de inspiração. Até uma próxima.

# Referências

#### Projeto completo

https://github.com/gustavohom/exemplos/tree/main/Corrida_de_barras

#### Inspiração

https://towardsdatascience.com/create-animated-bar-charts-using-r-31d09e5841da

#### Dados

https://snif.florestal.gov.br/pt-br/florestas-plantadas

https://iba.org/datafiles/publicacoes/relatorios/relatorio-iba-2020.pdf

#### Imagem de capa do artigo 

Floresta plantada - iStockFloresta plantada - iStock

https://www.gov.br/agricultura/pt-br/assuntos/noticias/mapa-desenvolve-programa-para-estimular-a-area-de-florestas-plantadas-no-territorio-nacional


