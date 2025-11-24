# Geração Procedural de Avatar — Documentação Completa

Este projeto contém a função `widget()`, responsável por gerar avatares procedurais em SVG de forma dinâmica e consistente a partir de uma *seed* (chave). O avatar é composto por várias camadas, como fundo, rosto, cabelo, olhos, boca e acessórios opcionais. Todas as informações abaixo foram extraídas e descritas com base nos arquivos presentes nas branches **main** e **master** do repositório.

---

# 🎯 Objetivo

Gerar automaticamente avatares SVG únicos e estáveis, utilizando um sistema de camadas, caminhos geométricos e escolhas baseadas em pseudoaleatoriedade controlada por uma chave (`key`). O avatar resultante é ideal para:

- Foto de perfil
- Representações visuais dinâmicas
- Sistemas de identificação gerados automaticamente
- Aplicações que requerem avatares consistentes baseados em um ID

---

# 🧠 Funcionamento da Função `widget(key, draw)`

A função recebe:

- **key:** objeto gerador pseudoaleatório (seeded)
- **draw:** instância do SVG.js usada para desenhar o avatar

O avatar é montado utilizando **grupos de camadas**, garantindo que os elementos sejam renderizados na ordem correta:

```js
const backLayer = draw.group();
const bodyLayer = draw.group();
const faceLayer = draw.group();
const accessory = draw.group();
```

---

# 🗂️ Estrutura por Camadas

## 1. 🎨 Fundo
É criado um círculo grande de fundo utilizando uma cor provida pelo iterador:

```js
const bgColor = nextColor();
backLayer.circle(900).center(500, 500).fill(bgColor);
```

## 2. 👤 Pele / Rosto
O rosto é um círculo centralizado, com a cor de pele escolhida de forma pseudoaleatória:

```js
let face = bodyLayer.circle(baseSize)
    .center(cx, cy - 80)
    .fill(skinColor);
```

---

# 💇‍♂️ Cabelo — 5 Estilos

A cor e o estilo do cabelo são gerados a partir da seed. Os possíveis estilos são:

1. Cabelo curto
2. Cabelo longo
3. Cabelo ondulado/topo destacado
4. Topete curvado
5. Careca

Cada estilo é desenhado usando `path()` ou `ellipse()`.

Exemplo (cabelo curto):

```js
faceLayer.path(`M${cx - 200},${cy - 210} 
    Q${cx},${cy - 300} ${cx + 200},${cy - 210} 
    L${cx + 200},${cy - 100} 
    Q${cx},${cy - 250} ${cx - 200},${cy - 100} Z`)
    .fill(hairColor);
```

---

# 👀 Olhos

São compostos por:

- Círculo branco do olho
- Pupila escura

```js
faceLayer.circle(50).center(...).fill("white");
faceLayer.circle(20).center(...).fill("#333");
```

A posição é calculada com base no centro do avatar.

---

# 🪄 Sobrancelhas

Criadas usando caminhos curvos:

```js
faceLayer.path(`M... Q... ...`)
    .stroke({ width: 8, linecap: "round" })
    .fill("none");
```

A cor acompanha a cor do cabelo.

---

# 👄 Boca — 3 Expressões

Dependendo da seed:

- 0 → sorriso  
- 1 → neutra  
- 2 → triste  

Exemplo (sorriso):

```js
faceLayer.path(`M${cx - 80},${cy + 40} 
    Q${cx},${cy + 90} ${cx + 80},${cy + 40}`);
```

---

# 👕 Roupa

A camiseta é desenhada com um grande path:

```js
bodyLayer.path(`M${cx - 120},${cy + 200}
    Q${cx},${cy + 280} ${cx + 120},${cy + 200}
    L${cx + 250},${cy + 450}
    L${cx - 250},${cy + 450} Z`)
    .fill(shirtColor);
```

---

# 🧢 Acessórios (opcionais)

Dependem do valor:

```js
const accessoryChance = key.next() % 4;
```

Tipos:

### 0 — Óculos
Criados com `rect()` + `line()`  
Desenha dois aros e uma ponte entre eles.

### 1 — Chapéu
Criado com dois retângulos:

```js
accessory.rect(500, 50).center(...)
accessory.rect(250, 70).center(...)
```

### 2 e 3 — Nenhum acessório

---

# 🔁 Determinismo e Reprodutibilidade

Como tudo é baseado na chave (`key`):

- A mesma seed sempre gera o mesmo avatar  
- Seeds diferentes produzem avatares totalmente distintos  
- Ideal para sistemas de usuário onde a chave é o ID do usuário

---

# 📦 Conclusão

A função `widget()` implementa um sistema completo de geração procedural de avatares em SVG utilizando:

- Geometria vetorial
- Paletas de cores
- Paths complexos
- Camadas de renderização
- Acessórios dinâmicos
- Personalizações controladas via seed

Esse sistema garante avatares:

- Bonitos
- Variados
- Determinísticos
- Fáceis de integrar em qualquer app

---

Gerado automaticamente com base nos arquivos das branches *main* e *master*.
