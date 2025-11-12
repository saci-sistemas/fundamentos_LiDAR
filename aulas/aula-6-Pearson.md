---
marp: true
theme: gaia
paginate: true
footer: "Favela3D+t · ALS LiDAR 3D temporal urbano"
title: "O novo vizinho: PCA e a consciência do espaço"
author: "Fernando Gomes"
math: katex
style: |
  section.small { font-size: 22px; }
  section.smedium { font-size: 26px; }
  section.tiny { font-size: 0.70em; }
  section.refs  { font-size: 0.80em; }
  section.refs .cols { columns: 2; column-gap: 2rem; }
---

# 🧭 PCA e a consciência do espaço  
> Da variância à forma  
#### Dedicada a Karl Pearson (1857–1936)
> que ensinou o espaço a se descrever,  
> e fez da variância um espelho da forma.  
>  
> Com ele, o ponto ganhou direções,  
> e a geometria, consciência.

---

## 🔁 Conexão com a aula anterior

> “Na aula passada, Shannon nos ensinou a filtrar o ruído.  
> Agora, Pearson nos ensina a **entender o que ficou**.”

A filtragem deixa o espaço **limpo**,  
o PCA faz o espaço **falar sobre si mesmo**.

---

## 🧮 O que é o PCA?

O **Principal Component Analysis** busca as **direções principais de variação** em um conjunto de pontos.

- Calcula a **matriz de covariância** da vizinhança de um ponto.  
- Extrai **autovalores (λ₁, λ₂, λ₃)** e **autovetores (e₁, e₂, e₃)**.  
- Cada autovetor → direção principal.  
- Cada autovalor → intensidade dessa variação.

> O PCA revela **como o espaço se espalha**.

---


## 🧩 O que é covariância?

> “Antes de se medir, o espaço precisa aprender a comparar.”

A **covariância** mede **como duas variáveis variam juntas**.

- Se aumentam **juntas** → covariância **positiva**  
- Se uma aumenta e a outra **diminui** → **negativa**  
- Se são **independentes** → covariância **zero**

---

## 🌀 O elipsoide de variação

Cada ponto pode ser visto como o **centro de um pequeno elipsoide**.

- **Autovetores:** eixos do elipsoide.  
- **Autovalores:** tamanhos desses eixos.  

| Tipo de forma | Relação entre λ | Estrutura local |
|----------------|----------------|----------------|
| Linear | λ₁ ≫ λ₂ ≈ λ₃ | Fio, cabo, borda |
| Planar | λ₁ ≈ λ₂ ≫ λ₃ | Telhado, parede, solo |
| Volumétrica | λ₁ ≈ λ₂ ≈ λ₃ | Vegetação, ruído |


---

## 💬 A metáfora

> “O PCA é o ouvido interno do LiDAR.  
> Ele não vê cores nem materiais,  
> mas sente como o espaço varia ao seu redor.  
>  
> Cada ponto é o centro de um elipsoide de variação —  
> e cada elipsoide, uma hipótese sobre o mundo.”

---

## 🧩 Features derivadas dos autovalores
<!-- _class: small -->
| Feature | Interpretação | Tipo de estrutura |
|----------|----------------|------------------|
| **Linearity** = (λ₁ − λ₂)/λ₁ | Direção dominante | Fios, galhos |
| **Planarity** = (λ₂ − λ₃)/λ₁ | Superfície definida | Telhados, calçadas |
| **Scattering** = λ₃/λ₁ | Dispersão tridimensional | Vegetação, ruído |
| **Anisotropy** = (λ₁ − λ₃)/λ₁ | Grau de coerência direcional | Estrutura bem definida |
| **Omnivariance** = (λ₁λ₂λ₃)^(1/3) | Volume do elipsoide | Complexidade local |
| **Eigenentropy** = −Σ λᵢ log λᵢ | Desordem da forma | Vegetação fina |
| **Surface Variation** = λ₃/(λ₁+λ₂+λ₃) | Curvatura local | Bordas, rugosidade |
| **Eigenvalue Sum** = λ₁+λ₂+λ₃ | Variância total | Energia geométrica |
| **Density** = N/V | Pontos por volume | Densidade local |
| **Verticality** = 1−\|n·z\| | Orientação da superfície | Chão vs fachadas |
| **Demantké Verticality** = \|arccos(\|n·z\|)\|/π | Inclinação contínua | Rampas, telhados |


---

## 📘 Escala e dimensionalidade (Demantké et al., 2011)
<!-- _class: smedium -->
> **Dimensionality-Based Scale Selection in 3D LiDAR Point Clouds**

- O tamanho da vizinhança muda o **formato do elipsoide**.  
- Pequenas escalas → detalhes e ruído.  
- Grandes escalas → superfícies amplas.  
- O método de Demantké escolhe **a escala ideal por ponto**,  
  maximizando a clareza entre **linear, planar ou volumétrico**.

🧭 *Métrica central:* “Demantké Verticality” —  
uma forma robusta e contínua de medir orientação.

---

## 🏙️ Segmentação e classificação (Guinard & Landrieu, 2017)
<!-- _class: smedium -->
> **Weakly Supervised Segmentation-Aided Classification of Urban Scenes from 3D LiDAR Point Clouds**

- Segmentam a nuvem em regiões **geométricamente homogêneas**.  
- Cada região herda atributos do PCA (planaridade, linearidade, anisotropia…).  
- Classificadores supervisionados (CRF) refinam os rótulos.  
- Reduzem a dependência de dados anotados.

💡 *Moral:*  
O PCA não é só análise — é **a base geométrica da inteligência urbana**.

---

## 🔬 Casos LiDAR

| Forma | Estrutura | PCA |
|-------|------------|-----------------------|
| Fios elétricos | Estrutura linear | λ₁ alto, λ₂≈λ₃ pequenos |
| Telhados / solo | Estrutura planar | λ₁≈λ₂ altos, λ₃ pequeno |
| Vegetação | Estrutura volumétrica | λ₁≈λ₂≈λ₃ |
| Fachadas verticais | Alta planaridade + verticalidade | λ₁≈λ₂≫λ₃; n·z≈0 |

---

## 🪶 Fechamento

> Pearson ensinou o espaço a olhar para si mesmo.  
>  
> Em cada ponto, o mundo se pergunta: em que direção eu vario mais?  
>  
> O ruído se torna estrutura. A variância, consciência.  
>  
> Agora que o espaço aprendeu a se descrever,  
> veremos, na próxima aula, **como ele conversa com seus vizinhos.**

---

## 📚 Referências

<section class="refs">
 
- Karl Pearson. *On Lines and Planes of Closest Fit to Systems of Points in Space.* Philosophical Magazine, 1901.  
- Jérôme Demantké, Clément Mallet, Nicolas David, Bruno Vallet. *Dimensionality-Based Scale Selection in 3D LiDAR Point Clouds.* ISPRS Laserscanning, 2011.  
- Stéphane Guinard, Loïc Landrieu. *Weakly Supervised Segmentation-Aided Classification of Urban Scenes from 3D LiDAR Point Clouds.* ISPRS Archives, 2017.  

</section>
