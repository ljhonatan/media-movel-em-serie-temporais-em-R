# media-movel-em-serie-temporais-em-R
media movel em serie temporais em R
# 📈 Médias Móveis em Séries Temporais (R)

Reprodução em código R dos conceitos apresentados no vídeo **"Séries Temporais e Análises Preditivas — O Curso Completo"**, focando na técnica de **Médias Móveis** para suavização de séries temporais.

---

## 📚 Sobre o conteúdo

O script demonstra, de forma prática e didática, os conceitos teóricos apresentados no vídeo:

- O que é uma **média móvel** e por que ela é usada
- Como funciona o conceito de **ordem** da média móvel
- O **cálculo manual** por trás da função (exemplo: `(76+67+15+56+13)/5 = 46`)
- O **efeito da ordem** no aparecimento de valores `NA` nas extremidades
- Aplicação prática no dataset **`CO2`** do R
- **Média móvel exponencial** como alternativa
- Comparação visual entre **diferentes ordens** de suavização

---

## 🗂️ Estrutura do script

| Seção | Descrição |
|-------|-----------|
| **1. Exemplo numérico** | Reproduz o cálculo manual do vídeo (ordem 5 → valor 46) e compara com `forecast::ma()` |
| **2. Efeito da ordem** | Demonstra como ordens maiores geram mais `NA` nas pontas (ordem 12 → 6 NAs em cada lado) |
| **3. Dataset CO2** | Aplica média móvel de ordem 2 na série `CO2` e plota original × suavizada |
| **4. Média exponencial** | Usa `ses()` para suavização exponencial simples |
| **5. Comparação de ordens** | Gera painel 2×2 com ordens 2, 6, 12 e 24 para evidenciar tendências |

---

## ⚙️ Requisitos

- **R** versão 4.0 ou superior
- Pacote **`forecast`**

Instalação do pacote:

```r
install.packages("forecast")
