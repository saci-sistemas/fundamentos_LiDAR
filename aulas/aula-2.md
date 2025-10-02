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

## 🔦 O que é o LASER?

- **Laser** = *Light Amplification by Stimulated Emission of Radiation*   
  - **Monocromático** (um comprimento de onda ~1064 nm (NIR))  
  - **Coerente** (ondas em fase)  
  - **Colimado** (feixe quase paralelo)  

📌 A **colimação** permite que o feixe viaje centenas de metros com pouca abertura.  
⚠️ Pela **difração**, sempre há **divergência** → footprint cresce com a altura.

---

## 📐 Divergência do laser

- Medida em **mrad** (miliradianos)  
  - 1 mrad = 1 m de abertura a cada 1000 m de distância  
- Datasheet indica **(1/e)** → largura onde intensidade cai para ~37%  
---

### Fórmula prática:

- Se θ for **half-angle**:

$$
D \approx D_0 + 2R\theta
$$

- Se θ for **full-angle**:

$$
D \approx D_0 + R\theta
$$


---

## 📊 Exemplo — São Paulo, 2017

**Sensor:** Optech Gemini  
**Parâmetros (Ribeiro, 2019):**  
- Frequência: 100–125 kHz  
- Ângulo de varredura: 18–25°  
- **Dual divergence:** 0.25 mrad (narrow) / 0.8 mrad (wide)  
- Altura média: 700 m  
- Densidade: ~10 pts/m² (esp. ~31 cm)

---


## 🧮 Footprint no solo

- **0.25 mrad (narrow):**

$$
D \approx 0.005 + 700 \times 0.00025 = 0.18\ \text{m}
$$

- **0.8 mrad (wide):**

$$
D \approx 0.005 + 700 \times 0.0008 = 0.56\ \text{m}
$$

➡️ Footprint varia entre **18 cm e 56 cm**.  
Comparável ao **espaçamento médio dos pontos (~31 cm)**.

---

## ⚖️ Implicações

- **Feixe estreito (0.25 mrad)** → maior resolução, captura detalhes finos.  
- **Feixe largo (0.8 mrad)** → footprint maior, mais energia, robusto em vegetação, mas menor separação de alvos.  

---

## ALS: Terminologia básica

- **Flight line** = trajetória da aeronave  
- **Scan line** = linha de pulsos  
- **Swath** = faixa coberta no terreno  
- **Scan angle** = ângulo lateral máx.  
- **Overlap** = redundância entre swaths  

---

## Linha de voo (*flight line*)

- Trajetória da aeronave durante o levantamento  
- Contém milhões de pulsos sequenciais  
- Importante no **planejamento**: direção, altitude, sobreposição  

📖 Wehr & Lohr (1999)

---

## Linha de varredura (*scan line*)

- Feixe não é só nadir → varredura lateral com espelhos rotativos 
- Cada movimento = uma **scan line**, quase ⟂ à flight line  
- Malha fundamental da coleta  

📖 Baltsavias (1999)

---

## Faixa de varredura (*swath*)

- Conjunto de scan lines → **faixa contínua no terreno**  
- Largura depende de altitude e ângulo máx.  
- Essencial para calcular **densidade (pts/m²)**  

📖 Shan & Toth (2008)

---

## Sobreposição de flight lines (*overlap*)

- Swaths vizinhos se sobrepõem (10–30%)  
- Garante:  
  - Cobertura completa (sem buracos)  
  - Redundância p/ calibração  
- Overlaps ↑ densidade local → requer normalização  

📖 Axelsson (2000)

---

> Gerar o MDS da favela São Remo com resolucão de 50cm, calculando o Z máximo e a densidade de pontos por píxel

```Python
pipeline_json = {
    "pipeline": readers + [
        {"type": "filters.merge"},
        {
            "type": "writers.gdal",
            "filename": "resultados/MDS_Sao_Remo_2020_max_altitude.tif",
            "resolution": resolucao_dsm,
            "output_type": ["max", "count"],
            "dimension": "Z",
            "gdaldriver": "GTiff"
        }
    ]
}
```

---

# 📚 Referências
<!-- _class: small -->
- BALTSAVIAS, E. (1999) — *Airborne laser scanning: basic relations and formulas*  
- WEHR, A.; LOHR, U. (1999) — *Airborne laser scanning — overview*  
- AXELSSON, P. (2000) — *DEM generation from laser scanner data*  
- SHAN, J.; TOTH, C. (2008) — *Topographic Laser Ranging and Scanning*
- RIBEIRO, Silvio César Lima. O uso do Lidar aerotransportado para obtenção de métricas de ocupação em assentamentos precários. Doutorado em Engenharia de Transportes—São Paulo: Universidade de São Paulo, 23 out. 2019.


