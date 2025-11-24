# Punto de Reorden (ROP) con Simulación Monte Carlo
### Cálculo del ROP cuando la demanda y/o el lead time no siguen distribuciones normales

---

## Descripción del proyecto

En la teoría clásica de inventarios, el **punto de reorden (ROP)** indica el nivel de inventario en el que debe emitirse un pedido para evitar quiebres de stock durante el **tiempo de entrega** (*lead time*).

Los modelos tradicionales del ROP asumen distribuciones **normales** tanto para la demanda como para el tiempo de entrega.  
En la práctica, esto rara vez ocurre: la demanda puede ser lognormal, gamma, Poisson, o presentar estacionalidad y tendencia; mientras que los tiempos de entrega suelen tener colas largas (Gamma, Lognormal, Weibull).

Este proyecto implementa métodos **no paramétricos** (ECDF, Bootstrap, Monte Carlo) para calcular el ROP sin asumir normalidad.

---

## Objetivos

- Calcular el ROP usando métodos determinísticos, probabilísticos y no paramétricos.
- Implementar la función de distribución empírica (ECDF).
- Implementar Bootstrap + Monte Carlo para demanda y lead time.
- Ofrecer código listo para uso empresarial en inventarios reales.
- Comparar métodos bajo estacionalidad, tendencia y varianza elevada.

---

# Modelos paramétricos clásicos

## 1. Modelo determinístico

$$ROP = \bar{L}\,\bar{D}$$

- \(\bar{L}\): lead time promedio  
- \(\bar{D}\): demanda promedio por unidad de tiempo  
- Nivel de servicio implícito ≈ **50%**

---

## 2. Modelo probabilístico normal

$$ROP = \bar{L}\bar{D} + z \sqrt{\bar{L}\sigma_D^2 + \bar{D}^2\sigma_L^2}$$

donde:

- \(\sigma_D^2\): varianza de la demanda  
- \(\sigma_L^2\): varianza del lead time  
- \(z\): cuantil normal del nivel de servicio deseado

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

$$d_{1}^{*},\dots,d_{B}^{*} \sim \{d_1,\dots,d_n\}$$

Lead time:

$$l_l^{*},\dots,l_B^{*} \sim \{l_1,\dots,l_m\}$$

Demanda durante el LT simulada:

$$D_{L,i}^{*} = d_i^{*}\,l_i^{*}$$

ROP no paramétrico:

$$ROP = \text{quantile}(D_L^{*}, SL)$$

---

# Método 3. Convolución Empírica (Independence Shuffle)

Si demanda y lead time son **independientes**:

1. Se permuta el vector de demanda.
2. Se permuta el vector de lead time.
3. Se generan miles de combinaciones posibles.
4. Se obtiene el cuantil del nivel de servicio.

---

