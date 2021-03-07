# Tabela Calendário

## Índice <!-- omit in toc -->

- [Tabela Calendário](#tabela-calendário)
  - [Sobre este Dataset](#sobre-este-dataset)
  - [Dicionário de Dados](#dicionário-de-dados)
  - [Tabela dCalendario Dinâmica (Script em DAX)](#tabela-dcalendario-dinâmica-script-em-dax)
    - [V1 com CALENDARAUTO()](#v1-com-calendarauto)
    - [V2 com CALENDAR()](#v2-com-calendar)
  - [Tabela dCalendário Estática (XLSX e CSV)](#tabela-dcalendário-estática-xlsx-e-csv)
  - [Fontes de inspiração deste Dataset](#fontes-de-inspiração-deste-dataset)
  - [Licença](#licença)
  - [Notas](#notas)

## Sobre este Dataset

A Tabela Calendário ("dCalendario") tem como objetivo servir de Dimensão que relacione duas ou mais tabelas Fato com colunas de data.

Dessa forma, você terá um range contínuo de datas (nem sempre presente nas colunas de datas das tabelas Fato), oque auxilia nas análises de dados temporais, bem como mantém a integridade e exatidão dos cálculos relativos à data. No Power BI, a Tabela Calendário inclusive facilita bastante o uso de [funções de inteligência de dados temporais em DAX](https://docs.microsoft.com/pt-br/dax/time-intelligence-functions-dax).

Além disso uma Tabela Calendário pode manter seu modelo de dados organizado e sucinto, evitando a criação de colunas desnecessárias (como por ex. ano, mês, trimestre etc.) dentro das tabelas Fato.

Aqui você encontra três formas diferentes de integrar a Tabela Calendário em seu modelo de dados:

1. Script DAX (recomendado para Power BI)
1. Arquivo .XLSX
1. Arquivo .CSV

[:top:](#tabela-calendário)

## Dicionário de Dados

A primeira coluna da Tabela Calendário é o campo chave do dataset, na qual cada linha corresponde a uma data (padrão dd/mm/yyyy). As demais colunas, da esquerda para a direita, seguem a hierarquia padrão de datas do Power BI: Ano > Trimestre > Mês > Semana > Dia, além de mais algumas colunas auxiliares para manipulação de datas.

Os datasets em DAX, .XLSX ou .CSV, foram desenvolvidos da melhor forma possível para agilizar o processo de carregamento no Power BI e evitar maiores transformações nos dados. Porém pequenos ajustes ainda podem ser necessários uma vez que os dados estejam carregados.

Para facilitar tais ajustes, temos a seguinte tabela de referência da base de dados:

| COLUNA | EXEMPLO | [TIPO DE DADO](https://docs.microsoft.com/pt-br/power-query/data-types) | FORMATO | NÚM. MÁX. DE CARACTERES | OBSERVAÇÃO |
| :--- | :--- | :---: | :--- | :---: | :--- |  
| *Data* | 21/10/2015 | ![Data](https://docs.microsoft.com/pt-br/power-query/images/date_20.png) | dd/mm/yyyy | 10 | Campo chave da tabela (Primary Key). |
| *Ano* | 2015 | ![Número Inteiro](https://docs.microsoft.com/pt-br/power-query/images/wholenumber_20.png) | Número Inteiro | 4 | - |
| *Trimestre* | 4T15 | ![Texto](https://docs.microsoft.com/pt-br/power-query/images/text_20.png) | Texto | 4 | Trimestre abreviado (padrão brasileiro). |
| *Quarter* | Q4/15 | ![Texto](https://docs.microsoft.com/pt-br/power-query/images/text_20.png) | Texto | 5 | Trimestre abreviado (padrão americano). |
| *Mês* | Outubro |  ![Texto](https://docs.microsoft.com/pt-br/power-query/images/text_20.png) | Texto | 9 | - |
| *Mês Abreviado* | Out |  ![Texto](https://docs.microsoft.com/pt-br/power-query/images/text_20.png) | Texto | 3 | - |
| *Número do Mês* | 10 | ![Número Inteiro](https://docs.microsoft.com/pt-br/power-query/images/wholenumber_20.png) | Número Inteiro | 2 | De 1 (Janeiro) a 12 (Dezembro). |
| *Mês e Ano* | Out/15 |  ![Texto](https://docs.microsoft.com/pt-br/power-query/images/text_20.png) | Texto | 6 | - |
| *ID do Mês* | 201510 | ![Número Inteiro](https://docs.microsoft.com/pt-br/power-query/images/wholenumber_20.png)  | Número Inteiro | 6 | Coluna para facilitar a ordenação de datas por ano e mês. Concatenação das colunas "*Ano*" com "*Número do Mês*". |
| *Semana do Ano*| 43 | ![Número Inteiro](https://docs.microsoft.com/pt-br/power-query/images/wholenumber_20.png) | Número Inteiro | 2 | De 1 a 54. As semanas começam às segundas-feiras (["Sistema 1" do Excel](https://support.microsoft.com/pt-br/office/n%c3%bamsemana-fun%c3%a7%c3%a3o-n%c3%bamsemana-e5c43a03-b4ab-426c-b411-b18c13c75340?ns=excel&version=90&syslcid=1046&uilcid=1046&appver=zxl900&helpid=xlmain11.chm60513&ui=pt-br&rs=pt-br&ad=br)). |
| *Dia do Mês* | 21 | ![Número Inteiro](https://docs.microsoft.com/pt-br/power-query/images/wholenumber_20.png) | Número Inteiro | 2 |De 1 a 31. |
| *Dia da Semana*| Quarta-Feira | ![Texto](https://docs.microsoft.com/pt-br/power-query/images/text_20.png) | Texto | 13 | - |
| *Dia da Semana Abreviado* | Qua |  ![Texto](https://docs.microsoft.com/pt-br/power-query/images/text_20.png) | Texto | 3 | - |
| *Número Dia da Semana* | 3 | ![Número Inteiro](https://docs.microsoft.com/pt-br/power-query/images/wholenumber_20.png) | Número Inteiro | 1 | De 1 (segunda-feira) a 7 (domingo). |
| *Dia e Semana* | 21-Qua |  ![Texto](https://docs.microsoft.com/pt-br/power-query/images/text_20.png) | Texto | 6 | Concatenação das colunas "*Dia do Mês*" e "*Dia da Semana Abreviado*". |
| *Dia Útil?* | 1 | ![Número Inteiro](https://docs.microsoft.com/pt-br/power-query/images/wholenumber_20.png) | Número Inteiro | 1 | Se é dia útil (1) ou não (0). Basta somar os números da coluna para saber quantos dias úteis tem no período. Feriados não estão considerados neste calendário. |

[:top:](#tabela-calendário)

## Tabela dCalendario Dinâmica (Script em DAX)

Os scripts em DAX abaixo proporcionam a criação automática de uma Tabela Calendário ("dCalendario") no Power BI.

### V1 com CALENDARAUTO()

A maior vantagem desta versão é que a dCalendario se ajustará dinamicamente conforme o range de datas que existir em outras tabelas de seu modelo de dados. Logo, se novas datas entrarem nas suas tabelas, a dCalendario criará automaticamente linhas de 01/jan até 31/dez entre o menor e o maior ano que existir em sua base. Mais detalhes [aqui](https://docs.microsoft.com/pt-br/dax/calendarauto-function-dax). Isso mantém o modelo de dados mais leve e elimina a necessidade de revisões manuais futuras na dCalendario.

Para usar esta versão, basta copiar o código abaixo e colá-lo na barra de fórmulas em uma nova tabela em branco (Guia "Modelagem" > botão "Nova Tabela")<sup id="voltar1">[[1]](#nota1)</sup>.

```dax
dCalendario = 
    ADDCOLUMNS(
        CALENDARAUTO(),
            "Ano",FORMAT([Date],"yyyy"),
            "Trimestre",CONCATENATE(CONCATENATE(FORMAT([Date],"q"),"T"),RIGHT(YEAR([Date]),2)),
            "Quarter",CONCATENATE(CONCATENATE("Q",FORMAT([Date],"q"))&"/",RIGHT(YEAR([Date]),2)),
            "Mês",UPPER(LEFT(FORMAT([Date],"mmmm"),1))&RIGHT(FORMAT([Date],"mmmm"),LEN(FORMAT([Date],"mmmm"))-1),
            "Mês Abreviado",UPPER(LEFT(FORMAT([Date],"mmm"),1))&RIGHT(FORMAT([Date],"mmm"),LEN(FORMAT([Date],"mmm"))-1),
            "Número do Mês",MONTH([Date]),
            "Mês e Ano",CONCATENATE(
                UPPER(LEFT(FORMAT([Date],"mmm"),1))&RIGHT(FORMAT([Date],"mmm"),LEN(FORMAT([Date],"mmm"))-1)&"/",RIGHT(YEAR([Date]),2)
                ),
            "ID do Mês",CONCATENATE(YEAR([Date]),FORMAT(MONTH([Date]),"00")),
            "Semana do Ano",WEEKNUM([Date],2),
            "Dia do Mês",DAY([Date]),
            "Dia da Semana",UPPER(LEFT(FORMAT([Date],"dddd"),1))&RIGHT(FORMAT([Date],"dddd"),LEN(FORMAT([Date],"dddd"))-1),
            "Dia da Semana Abreviado",UPPER(LEFT(FORMAT([Date],"ddd"),1))&RIGHT(FORMAT([Date],"ddd"),LEN(FORMAT([Date],"ddd"))-1),
            "Número Dia da Semana",WEEKDAY([Date],2),
            "Dia e Semana",CONCATENATE(
                DAY([Date])&"-",UPPER(LEFT(FORMAT([Date],"ddd"),1))&RIGHT(FORMAT([Date],"ddd"),LEN(FORMAT([Date],"ddd"))-1)
                ),
            "Dia Útil?",IF(WEEKDAY([Date],2)<=5,1,0)
    )
```

[:top:](#tabela-calendário)

### V2 com CALENDAR()

Em alguns casos, a dCalendario V1 pode apresentar um erro de "dependência circular" quando relacionada à outras tabelas do modelo. Para contornar esse erro, temos a dCalendario V2 que utiliza datas fixas por meio da função [CALENDAR()](https://docs.microsoft.com/pt-br/dax/calendar-function-dax).

Para usar esta versão, basta copiar o código abaixo e colá-lo na barra de fórmulas em uma nova tabela em branco (Guia "Modelagem" > botão "Nova Tabela")<sup id="voltar2">[[1]](#nota1)</sup>. Contudo, na linha que contém a função CALENDAR(), substitua o parâmetro "*Tabela[Coluna]*" na função [FIRSTDATE()](https://docs.microsoft.com/pt-br/dax/firstdate-function-dax) e [LASTDATE()](https://docs.microsoft.com/pt-br/dax/lastdate-function-dax) pela sua tabela e coluna que contenha o ano desejado como referência.

```dax
dCalendario = 
    ADDCOLUMNS(
        CALENDAR(DATE(YEAR(FIRSTDATE(Tabela[Coluna])),01,01),DATE(YEAR(LASTDATE(Tabela[Coluna])),12,31)),
            "Ano",FORMAT([Date],"yyyy"),
            "Trimestre",CONCATENATE(CONCATENATE(FORMAT([Date],"q"),"T"),RIGHT(YEAR([Date]),2)),
            "Quarter",CONCATENATE(CONCATENATE("Q",FORMAT([Date],"q"))&"/",RIGHT(YEAR([Date]),2)),
            "Mês",UPPER(LEFT(FORMAT([Date],"mmmm"),1))&RIGHT(FORMAT([Date],"mmmm"),LEN(FORMAT([Date],"mmmm"))-1),
            "Mês Abreviado",UPPER(LEFT(FORMAT([Date],"mmm"),1))&RIGHT(FORMAT([Date],"mmm"),LEN(FORMAT([Date],"mmm"))-1),
            "Número do Mês",MONTH([Date]),
            "Mês e Ano",CONCATENATE(
                UPPER(LEFT(FORMAT([Date],"mmm"),1))&RIGHT(FORMAT([Date],"mmm"),LEN(FORMAT([Date],"mmm"))-1)&"/",RIGHT(YEAR([Date]),2)
                ),
            "ID do Mês",CONCATENATE(YEAR([Date]),FORMAT(MONTH([Date]),"00")),
            "Semana do Ano",WEEKNUM([Date],2),
            "Dia do Mês",DAY([Date]),
            "Dia da Semana",UPPER(LEFT(FORMAT([Date],"dddd"),1))&RIGHT(FORMAT([Date],"dddd"),LEN(FORMAT([Date],"dddd"))-1),
            "Dia da Semana Abreviado",UPPER(LEFT(FORMAT([Date],"ddd"),1))&RIGHT(FORMAT([Date],"ddd"),LEN(FORMAT([Date],"ddd"))-1),
            "Número Dia da Semana",WEEKDAY([Date],2),
            "Dia e Semana",CONCATENATE(
                DAY([Date])&"-",UPPER(LEFT(FORMAT([Date],"ddd"),1))&RIGHT(FORMAT([Date],"ddd"),LEN(FORMAT([Date],"ddd"))-1)
                ),
            "Dia Útil?",IF(WEEKDAY([Date],2)<=5,1,0)
    )
```

[:top:](#tabela-calendário)

## Tabela dCalendário Estática (XLSX e CSV)

Esta versão estática da Tabela Calendário possui um range de datas pré-definido entre 01/01/1970 a 31/12/2070. O uso dessa versão é recomendado apenas em casos mais específicos onde não seja possível o uso do script DAX, pois a grande quantidade de linhas da tabela deixará o modelo bem mais pesado se carregada por inteiro (principalmente se seu modelo de dados possuir tabelas com datas móveis).

A dCalendario estática vem em dois "sabores":

- **[Arquivo .XLSX](https://github.com/andrecontisilva/Datasets/tree/main/Dim/dCalendario/xlsx)**, com fórmulas Excel embutidas nas células para manutenção ou refatoração do dataset, caso seja necessário inserir ou retirar novas linhas de datas<sup id="voltar3">[[2]](#nota2)</sup>.
- **[Arquivo .CSV](https://github.com/andrecontisilva/Datasets/tree/main/Dim/dCalendario/csv)**, para carregamento direto no Power BI sem a necessidade de maiores transformações. O CSV está em padrão UTF-8 separado por ";" (ponto e vírgula). Este arquivo nada mais é que a conversão em CSV do arquivo XLSX.

[:top:](#tabela-calendário)

## Fontes de inspiração deste Dataset

- ["O QUE É E PARA QUE SERVE A TABELA CALENDÁRIO?" - Laennder Alves](https://www.youtube.com/watch?v=xXD3w0vYDyU&ab_channel=LaennderAlves)
- ["TABELA CALENDÁRIO no POWER BI - Por que e como criar uma?" - Trinity Academia](https://www.youtube.com/watch?v=-IhgkCKIZFk&ab_channel=TrinityAcademia)
- ["Power BI (desktop) - dCalendário AUTOMÁTICO usando DAX" - Planilheiros](https://www.youtube.com/watch?v=IQl9J_KwrkE&ab_channel=Planilheiros)

[:top:](#tabela-calendário)

## Licença

> *MIT License*
>
> *Copyright (c) 2021 [André Conti Silva](https://www.linkedin.com/in/andreconti/)*

Para mais detalhes, veja a **[licença completa](https://github.com/andrecontisilva/Datasets/blob/main/LICENSE)**.

[:top:](#tabela-calendário)

## Notas

<b id="nota1">1:</b> *Por padrão, o DAX cria a dCalendario com a primeira coluna "**Data**" com o nome "**Date**". Seu nome deve ser renomeado manualmente para "**Data**" se esta for sua necessidade.* [↩](#voltar1) [↩](#voltar2)

<b id="nota2">2:</b> *Inserir linhas de datas **posteriores a 2070**??? Parabéns! Considere-se um Marty McFly ou Exterminador do Futuro...* [↩](#voltar3)

[:top:](#tabela-calendário)
