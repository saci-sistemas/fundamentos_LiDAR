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
math: katex
---

# ⚡ O Ruído e o Sinal  
## Como detectar a fiação aérea em São Paulo?  
> Filtragem e Informação  
#### Dedicada a Claude E. Shannon (1916 – 2001)

---

## Uma questão de classe

A classe *powerline* existe no padrão ASPRS,  
mas **não está calculada** nesta nuvem.  

👉 Então o que falta?

> Um **número**, um rótulo, uma decisão ou um conjunto de decisões `True|False`, simples assim!

---

## 💡 Dedicatória

> **Claude Elwood Shannon**  
> (1916 – 2001)  
> O homem que ouviu o ruído do mundo  
> e descobriu nele o sinal.

Inventor da **Teoria da Informação** (1948)  
— base de toda comunicação digital.

> “Informar é escolher o que importa.”

---

## ⚙️ O pipeline minimalista

```json
{
  "type":"filters.range",
  "limits":"ReturnNumber[1:4],NumberOfReturns![1:1],UserData[3:],Intensity[:10],Classification[19:19]"
}
````

Simples! Mas e o `O(n)`?

---

## ⏱️ Complexidade e Informação

Shannon não criou o `O(n)` —  
mas foi o primeiro a medir **o custo da informação**.

>Cada bit carrega um preço: _energia, tempo e atenção_.

---

## 🫆 "a" fórmula "do" artigo de Shannon

$$C = B \cdot \log_2(1 + \frac{S}{N})$$

| Símbolo | Significado                                   | Unidade / Analogia                                      |
| ------- | --------------------------------------------- | ------------------------------------------------------- |
| **C**   | *Capacidade do canal* (bits por segundo)      | A taxa máxima de informação possível sem erro           |
| **B**   | *Largura de banda* (Hz)                       | O quanto de espaço o canal tem para carregar informação |
| **S/N** | *Relação sinal-ruído* (Signal-to-Noise Ratio) | Quanto o sinal se destaca do ruído                      |


---

## 💭 O que realmente custa informar?

- **Executar** um filtro tem custo computacional → `O(n)`  
- **Transmitir** tem custo de canal → ($C = B \cdot \log_2(1 + \frac{S}{N})$)  
- **Compreender** tem custo cognitivo → _atenção, contexto, seleção_  

---

## 🌪️ Complexidade ≈ Entropia

>A cada ponto que filtramos, reduzimos a **entropia** do sistema.

$$H = - \sum p_i \log_2 p_i$$

Filtrar não é descartar — é substituir incerteza por estrutura.

>A complexidade é o custo de tornar o mundo compreensível. (Shannon, 1945) (parafraseado)

---

## 🎥 O problema real: segue o fio!

Repositório ➡ [`AndaSampa/fiacao-aerea`](https://github.com/AndaSampa/fiacao-aerea)  

Vídeo ➡ [YouTube](https://www.youtube.com/watch?v=NwZoCtJKs6U)

---

## 🛰️ O canal e o ruído

E se não forem milhares, mas **33 bilhões de pontos**?

>Mesmo com o **COPC**, que indexa o espaço,
porém ainda **não indexamos as outras dimensões**.

E se quisermos fazer isso em tempo real, na tela e visualizando?

---

## 🔮 Quando o espaço aprende a se filtrar

>Hoje filtramos com nossas regras, 
mas e outros fenômenos: vielas e fator de visão? 
Como filtrá-los, classificá-los e compará-los?

Próximas aulas: 
- vizinhança `k-nn` 
- auto-vetores e auto-valores (`PCA`)
- hierarquias espaciais (`octo-tree`)



