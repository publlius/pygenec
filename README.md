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



## Instalation

```bash
$ pip install pygenec
```

Or


```bash
$ python setup.py install
```



## Usage

```python
from numpy import exp, array, mgrid
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import axes3d
from matplotlib.animation import FuncAnimation


from pygenec.populacao import Populacao
from pygenec.selecao.roleta import Roleta
from pygenec.cruzamento.kpontos import KPontos
from pygenec.mutacao.flip import Flip
from pygenec.evolucao import Evolucao


def func(x, y):
    tmp = 3 * exp(-(y + 1) ** 2 - x **2)*(x - 1)**2 \
          - (exp(-(x+ 1) ** 2 - y **2) / 3 )\
          + exp(-x **2 - y ** 2) * (10 * x **3 - 2 * x + 10 * y ** 5)
    return tmp


def bin(x):
    cnt = array([2 ** i for i in range(x.shape[1])])
    return array([(cnt * x[i,:]).sum() for i in range(x.shape[0])])


def xy(populacao):
    colunas = populacao.shape[1]
    meio = int(colunas / 2)
    maiorbin = 2.0 ** meio - 1.0
    nmin = -3
    nmax = 3
    const = (nmax - nmin) / maiorbin
    x = nmin + const * bin(populacao[:,:meio])
    y = nmin + const * bin(populacao[:,meio:])
    return x, y


def avaliacao(populacao):
    x, y = xy(populacao)
    tmp = func(x, y)
    return tmp


genes_totais = 16
tamanho_populacao = 100

populacao = Populacao(avaliacao, genes_totais, tamanho_populacao)
selecao = Roleta(populacao)
cruzamento = KPontos(tamanho_populacao)
mutacao = Flip(pmut=0.9)
evolucao = Evolucao(populacao, selecao, cruzamento, mutacao)

evolucao.nsele = 10
evolucao.pcruz = 0.5


fig = plt.figure(figsize=(100, 100))
ax = fig.add_subplot(111, projection="3d")
X, Y = mgrid[-3:3:30j, -3:3:30j]
Z = func(X,Y)
ax.plot_wireframe(X, Y, Z)

x, y = xy(populacao.populacao)
z = func(x, y)
graph = ax.scatter(x, y, z, s=50, c='red', marker='D')

def update(frame):
    evolucao.evoluir()
    x, y = xy(populacao.populacao)
    z = func(x, y)
    graph._offsets3d = (x, y, z)

ani = FuncAnimation(fig, update, frames=range(10000), repeat=False)
plt.show()
```

