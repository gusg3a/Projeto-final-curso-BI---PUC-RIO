# COLOQUE O NOME DO PROJETO AQUI!!!

Aluno: Gustavo Araujo (https://github.com/gusg3a/Projeto-final-curso-BI---PUC-RIO)

Orientadora: Manoela Kohler (https://github.com/manoelakohler).

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

Arquivo Python utilizando o método FP_GROWTH:
BOP_Data_FP_Growth.ipynb

Arquivo Python utilizando o método APRIORI:
BOP_Data_apriori.ipynb

Arquivo excel do resultado do método FP_GROWTH:
associate_FP_0.01.csv

Arquivo excel do resultado do método APRIORI:
associate_apriori.csv

### Resumo

Os dados coletados a serem analisados são provenientes de um equipamento submarino chamado de BOP. 
Esses dados são coletados de forma assincronizada e possui enorme quantidade de atributos que são desconhecidos pelo usuário a analisa-los. O objetivo será conhecer e entender a relação de associação dos dados coletados aplicando-se o método de associação para fins de desenvolvimento de um simulador que possa se aproximar o possível do comportamento do BOP.

### 1. Introdução

Esta apresentação tem por finalidade servir como trabalho de conclusão do curso de “Business Intelligence” ministrado pela instituição PUC-RIO.
 O tema em questão foi escolhido pelo aluno com a finalidade da aplicação de uma técnica de “Data Mining” conhecida como “Associação” com o objetivo de agrupar dados desconhecidos e aprender a relação da associação desses dados.

A base de dados foi solicitada por cortesia de uma sonda de perfuração localizada no GOM e entre os dias 8 e 10 de dezembro foram realizados alguns testes periódicos do BOP enquanto estava instalado no leito marinho. Durante esses dias, o operador coletou os dados em formato.csv e enviou para a empresa a qual trabalho.
Os dados são coletados de forma assíncrona, quando ocorre qualquer mudança todos os atributos são registrados, ou seja, se a variável “002” mudou de valor no tempo x, todos os valores das outras variáveis também serão salvos nesse mesmo tempo x. Os arquivos.csv estão separados em de acordo com a categoria de equipamentos: (onde significa o fabricante do BOP e a numeração significa a quantidade de atributos contida no arquivo.csv).


### 2. Modelagem

Como a base de dados e bastante “pobre” e complexa, pois possui muitos atributos e a maioria desconhecidos, faz-se necessário uma limpeza de dados para eliminar atributos que não agregam valor algum. Já com todos os dados agrupados em um único dataframe, realiza-se a limpeza dos atributos que não possuem valores. Ocorre-se então a redução de 927 atributos para 246. Mais de 70% da base de dados não continha nenhum valor interessante durante o teste do BOP! Também pode-se verificar que os 20 atributos mais frequentes possuem 4% a 14% de valores presentes durante os testes, ou seja, percebe-se que a base é bastante escassa. Esses valores mais frequentes serão os mais enfatizados nos resultados finais.

Aplicação do Apriori
Com a base de dados mais limpa e mais compacta, foi utilizado o método de associação conhecido como APRIORI com a finalidade de obter a relação entre os atributos. Esse método foi escolhido dada sua vasta aplicação e simples implementação. 
Para a utilização do método a entrada de dados deve ser no formato de uma lista contendo os nomes dos atributos que possuem informação relevante no mesmo registro. Quando o atributo não tem informação, o mesmo não aparece no registro da lista, já quando o atributo obteve alguma informação relevante, aparece seu nome na lista de registro. Análogo a uma lista de mercado, quando houve a ocorrência de um item, por exemplo, aparece “Banana” no registro, nessa mesma lista, quando não há a ocorrência de outro item como “abacate”, nada é adicionado na lista. 
Porém, como o BOP não é uma lista de mercado, considerou-se que de um registro para o próximo, se houve mudança em um atributo, esse atributo tem seu valor convertido para o nome do registro. Caso o valor do registro anterior se mantenha o mesmo que o atual, o valor atual se torna “0”. Depois, converte-se esse dataframe em uma lista onde os valores nulos são removidos, ficando apenas os valores dos atributos em cada registro. 
Com a entrada adaptada e os parâmetros configurados, obteve-se o resultado dos atributos que possuem regras de associações mais frequentemente relacionadas, como uma lista de supermercado, os produtos que são comprados juntos.

Aplicação do FP_Growth
Com a base de dados mais limpa e mais compacta, foi utilizado o método de associação conhecido como FP_Growth com a finalidade de obter a relação entre os atributos. Esse método foi escolhido por ser considerado melhor preparado para grandes bases de dados. 
Para a utilização do método a entrada de dados deve ser no formato de um Dataframe contendo “0” e “1”. Quando o atributo não tem informação, “0”, já quando o atributo obteve alguma informação relevante, “1”. Análogo a uma lista de mercado, quando houve a ocorrência de um item, exemplo, banana, tem-se o valor “1” no atributo banana, nessa mesma lista, quando não há a ocorrência de outro item como abacate, leia-se “0” no atributo abacate. 
Porém, como o BOP não é uma lista de mercado, considerou-se que de um registro para o próximo , se houve mudança de valor em um atributo, esse atributo tem seu valor convertido para “1” no registro atual. Caso o valor do registro anterior se mantenha o mesmo que o atual, o valor atual se torna “0”. 
Com a entrada adaptada e os parâmetros configurados, obteve-se o resultado uma arvore chamada Frequent Pattern tree (FP-tree) onde se extrai os conjuntos de itens frequentes. Isso possibilita uma melhor eficiência na geração das regras de associação, pois evita constantes acessos na base de dados, diferentemente do APRIORI.

### 3. Resultados

Os resultados obtidos utilizando os dois métodos FP_Growth e APRIORI foram diferentes. 

Enquanto o resultado do método FP_GROWTH focou-se na associação de um menor grupo dos atributos mais frequentes existentes na base, trazendo menos informação útil. Já quando utilizamos o APRIORI e diminuímos o critério de confiabilidade e suporte mínimo, o resultado trouxe associações de variáveis que pouco registro possuem, porem as relações de causa e consequência ficaram redundantes a maioria das vezes trazendo por outro lado ruido para o resultado. 

Importantes considerações foram extraídas de ambos os resultados, como: 
Com o FP_Growth notou-se que o atributo mais frequente “60VDC Voltage Readout” está relacionado ao acionamento dos atributos que fazem parte da regulação de pressão aplicada no equipamento POD (parte do BOP que dentre uma das funções regula a pressão hidráulica aplicada para acionamento de válvulas proporcionais). 
Muitos atributos leem do mesmo sensor de pressão, porem tem os atributos com nomes diferentes. Importante para simplificar a implementação do simulador.

O modelo APRIORI (Quando a confiabilidade está bem ajustada assim como mínimo suporte) teve um resultado que trouxe mais associações mesmo que muita redundância. 

Muitas informações sobre as solenoides que são dispositivos atuados uma ou duas vezes durante o teste. Como a redundância entre atributos “SFS” e “SmV” que leem da mesma solenoide, porem grandezas diferentes. 
Válvulas muito próximas foram associadas, exemplo, válvulas vizinhas foram associadas, devido o acionamento quase imediato entre elas, mas como é conhecido o alinhamento dos testes, essa informação não foi relevante. 

Muitos atributos leem do mesmo sensor de pressão, porem tem os atributos com nomes diferentes. Importante para simplificar a implementação do simulador.


### 4. Conclusões


Com banco de dados obtido foi possível obter o resultado possível aplicando-se os métodos de associação , para futuros trabalhos recomenda-se um maior banco de dados com mais dias de exposição do equipamento, pois a taxa de “missing values” de todos atributos foram altas. Com maior numero de valores, as associações ficam de certa forma mais “ricas” e em grande proporção. 

Como aprendizado para futuros estudos, os dados coletados não tiveram tanta informação nessa amostra. O mesmo seria se fossemos ao mercado várias vezes, porém comprássemos no máximo um ou dois itens diversos por vez. Notou-se que para a riqueza da análise de associação é preciso não só diferentes compras, mas também compras em grandes quantidades. A mesma percepção para uma análise de associação para o BOP ser mais precisa, necessitaria de uma amostra de muitos testes em situações diferentes e com maior numero de ações por parte do equipamento.

Apesar do modelo APRIORI com a parametrização ótima ter obtido um resultado mais completo, ambos os modelos trouxeram informações distintas que quando usados juntos se completam. 

O modelo FP trouxe informações interessantes acerca dos atributos analógicos do equipamento. Por exemplo válvulas proporcionais que são acionadas através de um regulador de pressão foram mais visíveis no resultado desse modelo. Simplesmente pelo motivo da solenoide que atua esses reguladores de pressão ficam mais tempo “On” “off” como um ajuste e por isso tanto os atributos desse tipo de solenoide quanto a pressão desse tipo de válvula proporcional aparecem mais frequentemente no resultado desse modelo e fica claro sua associação. 

O modelo APRIORI foi mais além e trouxe em seu resultado também variáveis como as solenoides de válvulas que apenas abrem e fecham de maneira binária sem regulagem. Muitas dessas válvulas são atuadas poucas vezes e por isso “fogem” da análise do modelo FP, mas utilizando o “tuning” da parametrização do APRIORI consegue-se chegar a esse resultado mais profundo. 

Todos os resultados obtidos por ambos modelos serão de valor considerável para o desenvolvimento do simulador de BOP. 

---

Matrícula: 211.101.215

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*