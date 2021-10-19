---
title: "Instalando, atualizando e carregando pacotes no R de maneira eficiente"
date: '2021-09-18T08:06:25+06:00'
description: Pacote gmourao
output:
  html_document:
    df_print: paged
hero: r.png
menu:
  sidebar:
    name: Pacote {gmourao}
    identifier: r
    parent: linguagem-r
    weight: 40
---
  
Nesse artigo abordarei:
  
- Como  instalar, atualizar e carregar pacotes no R.
- Como tornar o processo eficiente.

---
## R base

Há algum tempo me perguntei: qual a maneira mais eficiente de instalar, carregar e atualizar um pacote no ambiente R? Essa é uma pergunta extremamente pertinente, principalmente quando vamos compartilhar nossos projetos. Quando feita de maneira errada ou insuficiente pode ocasionar alguns detalhes inconvenientes: (1) Falha no código devido ao carregamento errado ou falta dos pacotes necessários; (2) Número excessivo de linhas de código; (3) Difícil entendimento dos scripts; entre outros. Como exemplo:
  
```

#Instalando pacotes

install.packages("tidyverse")
install.packages("ggplot2")
install.packages("readxl")
install.packages("dplyr")
install.packages("tidyr")
install.packages("ggfortify")
install.packages("DT")
install.packages("reshape2")
install.packages("knitr")
install.packages("lubridate")

# Carregando pacotes


library(tidyverse)
library(ggplot2)
library(readxl)
library(dplyr)
library(tidyr)
require(ggfortify)
require(DT)
require(reshape2)
require(knitr)
require(lubridate)

```

## Usando funções e loops

Sei que muitos que lerão este texto não tem um aprofundamento na linguagem R, então deixarei um breve relato. Nunca tive um curso formal de R, aprendi a maior parte de forma autodidata, lendo livros, artigos, assistindo tutoriais, conversando com outros programadores, etc. Por muito tempo usei as funções de carregamento de pacotes sem entender como realmente funcionam. Sempre usei o **`require()`** , mas nunca me atentei sobre sua diferença em relação ao **`library()`**. Na verdade a mudança é bem sutil, mas gera resultados amplos. A função **`require()`** retorna um <span style="color:"red">**warning**</span> (um aviso), porém o aplicativo pode ser rodado normalmente, falhando apenas quando o pacote exigido (que causou o erro) for solicitado em uma função. O **`require()`** retorna um valor lógico <span style="color:"red">**FALSE**</span> ou <span style="color:"blue">**TRUE**</span>. Quando usamos a função **`library()`** para carregar um pacote, ele retornará um erro caso falhe, parando o aplicativo na linha do código onde o erro foi apresentado. Assim uma maneira eficiente para carregar os pacotes no ambiente R seria com a função:

```  
pkg <- require(tidyverse)

#Verifica se o pacote está instalado

if (pkg == FALSE) {
  #Instala pacotes ausentes
  
  install.packages("tidyverse")
  
}

#Carrega os pacotes


library(tidyverse)

```
A vantagem desse método é que ele verifica e (se necessário) instala os pacotes. Como desvantagem o código é extenso e quando passamos os valores para a função **`require()`** ele deve ser escrito como um objeto, porém a função **`install.packages()`** tem como parâmetro um vetor de ***character***. Podemos corrigir isso acrescentando o parametro <span style="color:"red">character.only</span> = <span style="color:"blue">TRUE</span> na função **`require()`** e **`library()`**. Quando temos um grande número de pacotes, esse método torna-se poluído. Com minha pequena experiência, aprendi que em alguns momentos a sua implementação não funciona (não sei o motivo). Uma segunda maneira de instalar e carregar os pacotes é usando o *loop* **`for()`**
  
Vamos usar como exemplo:
  
```  
packages <- c("tidyverse", "ggplot2", "readxl", "dplyr", "tidyr", "DT")

insPack <- function(packages) {
  for (i in 1:length(packages)) {
    #verifica se o pacote está instalado
    
    if (!require(packages[i], character.only = TRUE)) {
      #se o pacote não está instalado, irá instalá-lo
      
      install.packages(packages[i], dependencies = TRUE)
      
      #Carrega o pacote
      
      library(packages[i], character.only = TRUE)
            
    }}}


insPack(packages)

```
Essa função começa a facilitar nossa vida, mas ainda não é a melhor. Vamos então para uma maneira semelhante, porém que considero um pouco mais eficiente. Vamos verificar se os pacotes que queremos carregar estão na lista de pacotes já instalados no R e instalar os ausentes.

```

package <- c("tidyverse", "ggplot2", "readxl", "dplyr", "tidyr", "DT")

#Cria um vetor com o nome dos pacotes não instalados

is_installed <- package %in% rownames(installed.packages())

#Verifica os pacotes ausentes

if (any(is_installed == FALSE)) {
  #Instala os pacotes ausentes
  
  install.packages(package[!is_installed])
}

#Carregando todos os pacotes


lapply(package, library, character.only = TRUE)

```

Ainda, podemos carregar os pacotes dentro da função invisible caso desejemos ocultar os avisos:
  
```  
invisible(lapply(packages, library, character.only = TRUE))

```

## Pacotes especificos

Desde o primeiro código da publicação, até a função anterior, a forma de instalar e carregar tornou-se bem mais eficiente. Porém ainda não chegamos na melhor forma possível, pois também queremos a atualização dos pacotes se necessário. Alguns pacotes dizem fazer isto, não aprofundei em seus scripts, porém ao fazer alguns testes, não cheguei a um resultado satisfatório. Por vezes não carregaram, instalaram ou atualizaram os pacotes dos quais solicitei. Alguns pacotes e funções são:

```  
  
pacman::p_load()


librarian::shelf()

```

Deixarei alguns links ao final da publicação, com algumas fontes e o uso dos pacotes {pacman} e {librarian}.

## Minhas soluções

### Solução inicial

Então após algumas frustrações, decidi colocar a mão na massa (nesse caso, no teclado xD ). Comecei a pensar em soluções. Como tudo na vida, não precisamos recriar a roda, então usei os códigos anteriores como ponta pé inicial. Adaptei-os para o meu problema. Criei uma função da seguinte maneira:
  
``` 
pkg <- c("tidyverse", "ggplot2", "readxl", "dplyr", "tidyr", "DT")

m_load <- function(pkg) {
  ipkg <- c(pkg[!pkg %in% installed.packages()], pkg[pkg %in%
                                                       
                rownames(old.packages())])
  
  install.packages(ipkg)
  
  lapply(pkg, library, character = TRUE)
}


m_load(pkg)

```

Uma breve explicação da função: Primeiramente ela irá verificar se os pacotes estão ausentes da lista de pacotes instalados. Depois verificará se estão presentes na lista de pacotes desatualizados. Caso esteja em alguma das duas listas, o pacote será adicionado ao vetor ipkg. Em seguida serão instalados todos os pacotes presentes no vetor. Por fim serão carregados pelo loop "lapply".

Após criar a função, basta usar no script a função m_load( ) para carregar, instalar e atualizar uma lista de pacotes. Porém nem tudo são flores. Da maneira em que se encontra, a função aceita apenas um vetor do tipo character como parâmetro e só podem ser instalados pacotes do repositório CRAN.

### Solução final

As duas desvantagens foram corrigidas. Para primeira usei a função exists() para verificar se o atributo passado é um objeto já existente. Caso o retorno seja FALSE, então a função verifica se é um atributo do tipo character. Se os dois testes derem falsos, então converte-se o nome do objeto para um texto com a função deparse(). Para resolver a segunda desvantagem, diferenciar os pacotes no CRAN dos pacotes do GitHub, usei como inspiração parte do código do pacote librarian. Assim o código final ficou o seguinte:
 
```
m_load <- function(pkg) {
  if (exists(deparse(substitute(pkg)), where = .GlobalEnv)) {
    pkg <- pkg
  }
  
  else {
    if (try(is.character(pkg), silent = TRUE)
        == TRUE) {
      pkg <- pkg
      
    } else{
      pkg <- deparse(substitute(pkg))
      
    }
  }
  
  github <- grep("^.*?/.*?$", pkg, value = TRUE)
  
  githubNames <- sub("^.*?/", "", github)
  
  cran <- pkg[!(pkg %in% github)]
  
  pkg <- sub("^.*?/", "", pkg)
  
  ipkg <- c(pkg[!pkg %in% installed.packages()], pkg[pkg %in%
                                                       rownames(old.packages())])
  
  cranIns <- cran[which(cran %in% ipkg)]
  
  githubIns <- github[which(githubNames %in% ipkg)]
  
  if (length(cranIns) > 0) {
    install.packages(cranIns)
  }
  
  if (length(githubIns) > 0) {
    if ("remotes" %in% installed.packages() == FALSE) {
      install.packages("remotes")
    }
    
    remotes::install_github(githubIns)
  }
  
  lapply(pkg, library, character = TRUE)
}

```

## Maneira mais eficiente

Pronto, o código está finalizado. Ele ficou um pouco grande. Seria muito cansativo reescrevê-lo toda vez que fosse usá-lo. Assim criei um pequeno pacote que facilita seu uso. Para instalá-lo use:
  
```  
remotes::install_github("gustavohom/gmourao")

```

Depois de instalado, sempre que quiser basta usar uma das seguintes formas:
  
```

# 1 Nome de um pacote em character
  
  
m_load("beepr")


#2 uma variavel com nome de um pacote


pkg <- "beepr"


m_load(pkg)


#3 nome de dois pacotes em character


m_load(c("beepr", "rlang"))


#4 vetor com nome de dois pacotes


pkg <- c("beepr", "rlang")


m_load(pkg)


#5 todas a maneiras anteriores podem ser usadas com instalacao para
# github, para isso use: userName/package


pkg <- c("beepr", "gustavohom/gmourao")


m_load(pkg)


# 6 é possivel carregar o nome dos pacotes sem a necessidade de aspas,
# porem apenas um por vez


m_load(beepr)


m_load(gustavohom/gmourao)

```

## Considerações

Essa foi uma maneira simples que achei para carregar, instalar e atualizar todos os pacotes que preciso em um projeto. Isto facilitou o compartilhamento dos meus códigos, principalmente porque tive retornos ruins (falhas) dos pacotes que se propuseram a resolver essa questão.

Se chegou até aqui, me diga: Gostou da solução? Alguma sugestão para melhorá-la?
  
## Algumas referências:
  
https://github.com/gustavohom/gmourao/blob/main/R/m_load.R

https://statsandr.com/blog/an-efficient-way-to-install-and-load-r-packages/
  
https://towardsdatascience.com/fastest-way-to-install-load-libraries-in-r-f6fd56e3e4c4

https://medium.com/@ashirwad1992/are-you-installing-and-loading-r-packages-the-efficient-way-d1782409b90e

https://github.com/DesiQuintans/librarian/blob/master/R/exported_functions.R
