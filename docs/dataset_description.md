# Estrutura do Dataset

## Usuários

Cada registro de usuário possui os seguintes atributos:

- **user_id** — identificador único do usuário
- **age** — idade do usuário
- **gender** — gênero do usuário
- **lon e lat** — longitude e latitude do usuário
- **preferences** — conjunto de categorias de interesse do usuário, juntamente com seus respectivos pesos de preferência

## Anúncios

- **ad_id** — identificador único do anúncio
- **lon_target e lat_target** — longitude e latitude do anúncio (evento)
- **location_sensitive** — indica se a distância importa
- **age_target** — faixa etária alvo do anúncio
- **gender_target** — gênero alvo da campanha
- **categories** — categorias associadas ao anúncio
- **bid** — valor que o anunciante está disposto a pagar por interação ou exibição
- **budget** — orçamento total disponível para o anúncio
- **trust_score** — pontuação de confiabilidade do anunciante na plataforma
- **estimated_conversion_rate** — taxa estimada de conversão do anúncio
- **click_probability** — probabilidade de clique

# Domínios dos Atributos

Esta seção descreve os valores possíveis utilizados na geração de alguns atributos do dataset.

## Categorias

As categorias representam **interesses de usuários** e **temas dos anúncios** dentro do domínio de jogos. As categorias utilizadas no dataset são:

- FPS
- Battle_Royale
- MOBA
- RPG
- MMORPG
- Strategy
- Simulation
- Sports_Games
- Racing
- Indie_Games
- Retro_Gaming
- Mobile_Gaming
- PC_Gaming
- Console_Gaming
- Game_Development
- Game_Streaming
- Esports
- Game_Hardware
- VR_Gaming
- Open_World

Além disso, algumas categorias representam **jogos específicos populares**:

- League_of_Legends
- Fortnite
- Minecraft
- Counter_Strike
- World_of_Warcraft

Essas categorias são utilizadas tanto para representar **interesses dos usuários** quanto para definir **o público alvo dos anúncios**.

## Gênero

O atributo **gender** pode assumir os seguintes valores:

- **M** — masculino
- **F** — feminino
- **Any** — sem preferência (apenas para ads)

Esse atributo é utilizado tanto nos **usuários** quanto na **segmentação de anúncios**.

## Estados

Os estados utilizados no dataset são:

- SP — São Paulo
- MG — Minas Gerais
- RJ — Rio de Janeiro
- BA — Bahia
- PR — Paraná

# Distribuição dos Dados

## Usuários

Para aproximar os dados da realidade, os usuários foram divididos em três grupos com base na quantidade de categorias pelas quais demonstram interesse: **usuários nichados** (2 a 4 categorias), **usuários médios** (5 a 8 categorias) e **usuários amplos** (9 ou mais categorias).

A distribuição desses grupos no dataset segue aproximadamente a proporção **6:3:1**, ou seja, a maioria dos usuários possui poucos interesses principais, enquanto uma parcela menor apresenta interesses mais diversos.

Em relação aos **pesos das preferências**, foi utilizada a **distribuição de Dirichlet**, que gera pesos aleatórios normalizados para as categorias selecionadas.

## Anúncios

Para alguns atributos dos anúncios também foram utilizadas distribuições específicas, com o objetivo de tornar os dados mais próximos de cenários reais de plataformas de anúncios.

### Categorias

Os anúncios foram divididos em três grupos de acordo com a quantidade de categorias alvo:

- **Anúncios nichados (1–2 categorias)** — compõem aproximadamente **50%** do dataset
- **Anúncios médios (3–6 categorias)** — compõem aproximadamente **30%** do dataset
- **Anúncios amplos (mais de 6 categorias)** — compõem aproximadamente **20%** do dataset

### Bid

O **bid** segue uma **distribuição log-normal**, com valores concentrados próximos de **20** e um valor mínimo de **15**. Isso significa que a maior parte dos anúncios possui bids próximos desse valor, enquanto bids mais altos aparecem com menor frequência.

### Budget

O **budget** é definido a partir do bid do anúncio. Para cada anúncio, o bid é multiplicado por um fator aleatório entre **100, 500 ou 1000**, gerando o orçamento total da campanha.

### Trust Score

A distribuição do **trust score** foi definida da seguinte forma:

- **70% a 100% de confiança** — aproximadamente **70%** dos anúncios
- **40% a 70% de confiança** — aproximadamente **20%** dos anúncios
- **menos de 40% de confiança** — aproximadamente **10%** dos anúncios

Essa distribuição simula um cenário onde a maioria dos anunciantes possui boa reputação na plataforma.

### Taxa de Conversão

A **taxa de conversão estimada** é calculada usando a seguinte fórmula:

```
taxa de conversão = base × fator de nicho × confiança
```

Onde:

- **Base** representa a taxa média de conversão, variando entre **1% e 5%**
- **Fator de nicho** depende da quantidade de categorias alvo do anúncio:
  - se o anúncio tiver **até 2 categorias**, o fator é **1,2**
  - caso contrário, o fator é **0,8**

### Probabilidade de Clique

A **probabilidade de clique estimada** é calculada utilizando a seguinte fórmula:

```
probabilidade de clique = base × fator de nicho × confiança
```

Onde:

- **Base** representa a taxa média de cliques (_CTR base_), gerada aleatoriamente entre **1% e 5%**.
- **Fator de nicho** depende da quantidade de categorias alvo do anúncio:
  - se o anúncio tiver **até 2 categorias**, o fator é **1,2**
  - caso contrário, o fator é **0,8**

- **Confiança** corresponde ao **trust score** do anunciante, um valor entre **0 e 1** que representa a confiabilidade do anúncio.

Essa fórmula simula o comportamento de anúncios mais **segmentados e confiáveis**, que tendem a apresentar uma **maior probabilidade de clique**.
