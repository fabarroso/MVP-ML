# MVP-ML

MVP - Machine Learning

Item 3 - Dados: carga, entendimento e qualidade  
    Foi verificado que os estavam com todos os campos vazios. Essas linhas foram excluídas, pois, não continham registros válidos. Ao remover essas linhas vazias, observamos melhora na qualidade dos dados utilizados para análise, evitando que valores ausentes ou irrelevantes distorçam os resultados.
    A análise preliminar do dataset foi realizada com o objetivo de entender sua estrutura e identificar potenciais inconsistências nos tipos de dados e na presença de valores ausentes. O comando df.info() forneceu um resumo detalhado das colunas, tipos de dados e a quantidade de valores não nulos, permitindo verificar se o conjunto de dados estava devidamente formatado. Além disso, a criação do DataFrame column_structure ofereceu uma visão organizada e legível das colunas, suas descrições e tipos de dados. Essa abordagem facilitou a identificação de colunas com nomes mais legíveis e a confirmação de que os tipos de dados estavam corretos. O dataset contém variáveis relacionadas a crimes e vitimizações no estado de São Paulo, com colunas que incluem informações de homicídios, roubos, furtos, entre outros. 
