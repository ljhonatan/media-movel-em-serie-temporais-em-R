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


# ============================================================
# MÉDIAS MÓVEIS - Reprodução do conteúdo do vídeo
# ============================================================

# Pacotes necessários
# install.packages("forecast")
library(forecast)

# ------------------------------------------------------------
# 1) EXEMPLO NUMÉRICO DO VÍDEO (ordem 5, valor em março = 46)
# ------------------------------------------------------------
# Série hipotética do exemplo (jan, fev, mar, abr, mai, jun...)
serie_exemplo <- c(76, 67, 15, 56, 13, 20, 30, 40)
# Índices:        1   2   3   4   5   6   7   8
# Em "março" (posição 3), com ordem 5, usamos:
#   2 valores para trás (pos 1, 2), o próprio (pos 3) e 2 para frente (pos 4, 5)
calc_manual <- (76 + 67 + 15 + 56 + 13) / 5
cat("Cálculo manual (março, ordem 5):", calc_manual, "\n")  # 46

# Aplicando média móvel de ordem 5 (função do pacote forecast)
mm5 <- ma(serie_exemplo, order = 5, centre = TRUE)
print(round(mm5, 2))
# Observação: com ordem 5 os extremos ficam NA (não há valores suficientes)

# ------------------------------------------------------------
# 2) DEMONSTRAÇÃO DO EFEITO DA ORDEM (NA nas pontas)
# ------------------------------------------------------------
cat("\n--- Ordem 2 ---\n")
print(ma(serie_exemplo, order = 2))

cat("\n--- Ordem 5 ---\n")
print(ma(serie_exemplo, order = 5))

# Com ordem 12 (exemplo citado no vídeo: 6 meses NA no início/fim)
serie_maior <- ts(rnorm(24, 50, 10), frequency = 12)
cat("\n--- Ordem 12 (6 NAs em cada ponta) ---\n")
print(ma(serie_maior, order = 12))

# ------------------------------------------------------------
# 3) EXEMPLO PRÁTICO COM O DATASET CO2 (ordem 2)
# ------------------------------------------------------------
data(CO2)
co2_ts <- CO2  # série temporal mensal já disponível no R

# Média móvel de ordem 2 (como no vídeo)
co2_mm2 <- ma(co2_ts, order = 2, centre = TRUE)

# Visualização: série original x série suavizada
plot(co2_ts,
     main = "CO2 - Série original vs Média Móvel (ordem 2)",
     ylab = "CO2 (ppm)", xlab = "Tempo",
     col = "gray50", lwd = 1)
lines(co2_mm2, col = "red", lwd = 2)
legend("topleft",
       legend = c("Original", "Média Móvel ordem 2"),
       col = c("gray50", "red"), lwd = c(1, 2))

# ------------------------------------------------------------
# 4) MÉDIA MÓVEL EXPONENCIAL (citada no vídeo)
# ------------------------------------------------------------
co2_ema <- ses(co2_ts, h = 12)   # suavização exponencial simples
plot(co2_ema, main = "Suavização Exponencial - CO2")

# ------------------------------------------------------------
# 5) COMPARAÇÃO DE DIFERENTES ORDENS (tendência mais clara)
# ------------------------------------------------------------
par(mfrow = c(2, 2))
for (ord in c(2, 6, 12, 24)) {
  plot(co2_ts, col = "gray70", main = paste("Média Móvel ordem", ord),
       ylab = "CO2", xlab = "Tempo")
  lines(ma(co2_ts, order = ord, centre = TRUE), col = "blue", lwd = 2)
}
par(mfrow = c(1, 1))
