# Cálculo no paramétrico del Punto de Reorden (ROP) mediante ECDF, Bootstrap y Simulación Monte Carlo

---

## Descripción del proyecto

En la teoría clásica de inventarios, el **punto de reorden (ROP)** indica el nivel de inventario en el que debe emitirse un pedido para evitar quiebres de stock durante el tiempo de entrega (*lead time*).

Los modelos tradicionales del ROP asumen distribuciones normales tanto para la demanda como para el tiempo de entrega. En la práctica, esto rara vez ocurre: la demanda puede ser lognormal, gamma, Poisson, o presentar estacionalidad y tendencia; mientras que los tiempos de entrega suelen tener colas largas (Gamma, Lognormal, Weibull).

Este proyecto implementa métodos no paramétricos (ECDF, Bootstrap, Monte Carlo) para calcular el ROP sin asumir normalidad.

---

## Objetivos

- Calcular el ROP usando métodos determinísticos, probabilísticos y no paramétricos.
- Implementar la función de distribución empírica (ECDF).
- Implementar Bootstrap + Monte Carlo para demanda y lead time.
- Comparar métodos bajo estacionalidad, tendencia y varianza elevada.

---

# Modelos paramétricos clásicos

## 1. Modelo determinístico

$$ROP = \bar{L} \times \bar{D}$$

- $\bar{L}$: lead time promedio  
- $\bar{D}$: demanda promedio por unidad de tiempo  
- Nivel de servicio implícito ≈ **50%**

---

## 2. Modelo probabilístico normal

$$ROP = \hat{D}_{L} + SS$$

$$ROP = \bar{L}\bar{D} + z \sqrt{\bar{L}\sigma_D^2 + \bar{D}^2\sigma_L^2}$$

donde:

- $\hat{D}_{L}$: demanda estimada durante el *lead time*
- $SS$: inventario de seguridad (*safety inventory*)
- $\sigma_D^2$: varianza de la demanda  
- $\sigma_L^2$: varianza del lead time  
- $z$: cuantil normal del nivel de servicio deseado

**Limitación:** funciona solo si ambas variables son normales e independientes.

---

# Métodos No Paramétricos

Cuando demanda y lead time **no** son normales, se usan métodos empíricos.

---

# Método 1. Función de Distribución Empírica (ECDF)

La demanda durante el lead time se define como:

$$D_{L,i} = L_i \cdot D_i$$

La ECDF es:

$$\hat{F}_n(x) = \frac{1}{n}\sum_{i=1}^{n}1\{D_{L,i} \le x\}$$

El ROP es el **cuantil empírico**:

$$ROP = \hat{F}_n^{-1}(SL)$$

---

# Método 2. Bootstrap + Simulación Monte Carlo

### Si hay pocos datos, se remuestrea con reemplazo:

Demanda:

$$d_1^{\ast}, \dots, d_B^{\ast} \sim \{d_1, \dots, d_n\}$$

Lead time:

$$
l_1^{\ast}, \dots, l_B^{\ast} \sim \{l_1, \dots, l_m\}
$$

Demanda durante el LT simulada:

$$D_{L,i}^{\ast} = d_i^{\ast} \cdot l_i^{\ast}$$

ROP no paramétrico:

$$ROP = \text{quantile}(D_L^{\ast}, SL)$$

---

### Método 3. Convolución Empírica (Independence Shuffle)

Si demanda y lead time son **independientes**:

1. Se remuestrea la demanda con reemplazo (**bootstrap de** $D$).
2. Se remuestrea el lead time con reemplazo (**bootstrap de** $L$).
3. Se generan miles de combinaciones independientes $D^{\ast} \times L^{\ast}$ mediante **Monte Carlo**.
4. Se obtiene el **cuantil empírico** del nivel de servicio objetivo a partir de la distribución simulada de $D^{\ast}L^{\ast}$.


---

