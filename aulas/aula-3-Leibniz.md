---
marp: true
theme: gaia  
paginate: true
footer: "Favela3D+t, ALS LiDAR 3D temporal urbano"
title: "Fundamentos do LiDAR"
author: "Fernando Gomes"
style: |
  /* classes para usar em slides específicos */
  section.small  { font-size: 28px; }   /* ~85% do tamanho padrão */
  section.tiny   { font-size: 0.70em; }   /* ~70% do tamanho padrão */
  section.refs   { font-size: 0.80em; }
  section.refs .cols { columns: 2; column-gap: 2rem; } /* duas colunas no slide */
---

⚫ Ponto?

- não tem extensão, nem forma, nem cor.
- o lugar onde o ser toca o espaço.
- inicio e fim de todas as linhas
- encontro de infitas retas e trajetórias
- mas e quando ele ganha um referencial?

---

## 🪞 As mônadas de Leibniz

> "Cada substância simples é um espelho vivo do universo."

- Mônadas: unidades sem extensão, mas com percepção.
    
- Cada uma contém o todo sob um ponto de vista próprio.
    
- O mundo como uma rede de centros de experiência.
    

🧠 **Ideia-chave:** o real é relacional.

---

## 💡 Do contínuo ao discreto

- No LiDAR, o mundo é medido em **pontos discretos (x, y, z)**.
    
- Cada ponto é uma **mônada geométrica**: contém informação local do todo.
    
- Para trabalhar matematicamente, tratamos cada ponto como um **vetor com vizinhança fixa**.
    

---

## 📍 Vetores com vizinhança fixa

- Cada ponto conhece apenas seu entorno imediato.
    
- Estruturas usuais:
    
    - k-nearest neighbors (k-NN)
        
    - busca por raio
        
    - KD-Tree / voxel grid
        

🧭 **A vizinhança é o mundo local da mônada.**

---

## 🏔️ Classificadores de terreno

Antes de modelar o campo, precisamos isolar o solo:

- `filters.smrf` — _Simple Morphological Filter_
    
- `filters.pmf` — _Progressive Morphological Filter_
    
- `filters.csf` — _Cloth Simulation Filter_
    

🔎 Todos estimam uma **superfície base** (pontos de solo), sem interpolar ainda.

🧩 Vamos explorar esses classificadores em detalhe em aulas futuras.

---

## 🧭 O problema fundamental

Transformar o discreto em contínuo:

$$f(x, y) = z \quad \Rightarrow \quad \nabla f(x,y) = \text{campo vetorial de declividade}$$


- $f(x, y)$: campo escalar de altitude.
    
- $∇f(x, y)$: campo vetorial (declividade + direção).
    

📏 **O nabla (∇)** é o operador que revela como a superfície varia.

---

## ⚪ Três tipos de campo

|Tipo|Representa|Exemplo no LiDAR|
|---|---|---|
|Escalar|valor único|altitude, intensidade|
|Vetorial|direção + intensidade|declividade, fluxo|
|Tensorial|relação entre direções|curvatura, anisotropia|

🪶 Campo = função que atribui um valor (ou vetor) a cada ponto.

---

## ⚙️ O desafio computacional

- Nuvens LiDAR → milhões de pontos.
- Comparar cada ponto com todos → **O(n²)**. 🚫

**Soluções:**

- vizinhança local (k-NN, raio)
- estruturas KD-Tree / voxel grid
- regressão local / média ponderada
    
💬 “Cada gradiente é local — juntos formam o relevo global.”

---

## 🧮 Métodos de interpolação (ponto → raster)

|Método|Tipo|Onde é útil|
|---|---|---|
|**TIN (Delaunay)**|Geométrico|áreas urbanas, degraus|
|**IDW**|Determinístico|superfícies suaves|
|**Spline / RBF**|Determinístico|campos contínuos|
|**Krigagem**|Geoestatístico|incerteza e suavidade|
|**Natural Neighbor**|Geométrico|transições suaves|
|**MLS**|Geométrico|reconstrução 3D|

---

## 🧰 Implementações no PDAL

|Filtro|O que faz|~= aproximado|
|---|---|---|
|`filters.delaunay`|triangulação Delaunay|TIN|
|`filters.faceraster`|rasteriza a triangulação|TIN → raster|
|`writers.gdal`|interpolação média ou IDW|raster simples|
|`filters.zsmooth`|suavização local|spline leve|
|`filters.python`|integração customizada|livre|

PDAL cobre o essencial, mas permite **extensão via Python**.

---

## 🧩 As bibliotecas externas

|Biblioteca|Paradigma|potencial|
|---|---|---|
|**SciPy**|Determinístico|Interpolação local rápida|
|**PyKrige**|Geoestatístico|Krigagem e variância|
|**GSTools**|Simulação|Campos correlacionados|
|**Open3D**|Geom. 3D|Vizinhança e curvatura|
|**WhiteboxTools**|GIS|Processamento rápido e hidrologia|

Essas libs expandem o universo PDAL.

---

## 🧪 O laboratório de modelagem de terreno

**Objetivo:** ir das mônadas (pontos) ao campo (raster).

1. Pré-processar e classificar o terreno (PDAL).
    
2. Criar o campo contínuo (TIN, IDW, krigagem).
    
3. Analisar o gradiente (∇f) e curvatura (tensor).
    
4. Explorar bibliotecas complementares.
    

🧠 _Do ponto que percebe ao campo que emerge._

---
<!-- _class: small -->
### 🕯️ **Dedicatória** _a Gottfried Wilhelm Leibniz (1646–1716)_
> 
> Que viu no ínfimo a totalidade,  
> no ponto o espelho do cosmos,  
> e na harmonia das partes  
> a respiração secreta do todo.
> 
> Que nos ensinou  
> que nada está isolado,  
> e que até um grão de poeira  
> contém o rumor do universo.
> 
> Hoje,  
> quando olhamos o relevo através de milhões de pontos,  
> é ainda sua voz que nos fala.