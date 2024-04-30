python -m pip install virtualenv

virtualenv -p python3.12 env


Em resumo o que nosso AG vai realizar em etapas é:
1. criar população inicial;
2. determinar os indivíduos mais adaptados;
3. cruzamento e procriação;
4. mutação/evolução propriamente dita;

1. População
Vamos determinar o número de cromossomos e genes que sâo capazes de representar uma possível solução para um dado problema.
Devemos considerar que precisamos encontrar o máximo de uma função
f(x,y) e então teremos os cromossomos a serem utilizados.

Exemplo:
Para x determinar o máximo da função para valores que estejam entre 1 e 5.
para y determinar o máximo da função para valores que estejam entre 10 e 20.

Assumindo que utilizaremos 8 bits para cada cromossomo, no caso de x o menor intervalo de busca será dado por (5-1) / 256 = 0,015625. (0 a 255).

Para o critério de seleção, quanto mais genes tiver um cromossomo, melhor será a busca para tal intervalo.


2.Seleção
Cada indivíduo pode ser definido em uma solução do problema, logo precisamos escolher os melhores.
É possível definir probabilidades diferentes para que cada indivíduo seja selecionado conforme a solução ótima.
O valor numérico da qualidade do indivíduo é chamado de fitness e quanto maior o fitness, maior será a capacidade de procriação.

3. Cruzamento
Este passo consiste em combinar cadeias genéticas, onde dois indivíduos são selecionados e as cargas genéticas combinadas gerando dois novos filhos. Tal procedimento deve ser repetido até que o número total de filhos/novos indivíduos seja igual a população original menos um. Este é o modelo elitista e os genes do melhor indivíduo da população antiga são mantidos inalterados na nova.

4. Mutação
Por meio de pequenas mudanças é que novas características são introduzidas e em alguns casos pode acarretar na melhor adaptabilidade de uma determinada espécie ao meio, vale ressaltar que a maior parte da mutação não será benéfica, porém todo novo indivíduo está sujeito a alguma alteração em seu código genético. Nesta etapa os indivíduos são escolhidos aleatóriamente para que sofram alguma mutação.

Evolução
Consiste em aplicar os passos seleção, cruzamento e mutação para cada nova população durante um número predefinido (pelo desenvolvedor) de iterações.
Ao final de cada ciclo deseja-se ter encontrado os indivíduos que melhor representam a solução para o problema.

			1. Criação da população inicial

			2. Realizar primeira avaliação

			3. Enquanto t < tfinal repita:
				3.1. Selecionar indivíduos;
				3.2. Realizar cruzamento;
				3.3. Mutações controladas;
				3.4. Avaliar a população;

			4. Escolher melhor solução

