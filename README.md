# TAREA-2-FUNDAMENTO-DE-PROCESAMIENTO-DE-IM-GENES
**Autor:** [Martina Suárez]
**Curso:** [Fundamentos del procesamiento de imágenes]
**Fecha:** [09/10/2026]

## Descripción
En este repositorio hay una guia para ver la resolución de la Tarea 2, que consiste en dos partes, cada una correspondiente a una pregunta:

1. **Pregunta 1:** Filtrado Gaussiano — kernel isotrópico y kernel adaptativo por intensidad.
2. **Pregunta 2:** Difusión anisotrópica — coeficiente TV y dos coeficientes alternativos (TV modulado por Laplaciano y exponencial gradiente + Laplaciano).

Las dos preguntas se desarrollaron en Python, originalmente mediante VScode, posteriormente estructurada en Google Colab para mayor claridad, usando las siguentes librerias: `numpy`, `matplotlib`, `scipy` y `cv2.

## Pregunta 1 - Filtro Gaussiano 

El objetivo consistió en implementar dos métodos de filtrado en una imagen creada (fondo + cuadrado + circulo) agregándole ruido Poisson.

Primero se implementó un kernel Gaussiano isotrópico con un único valor sigma. Para el segundo filtro, se diseñó un kernel gaussiano adaptativo según la intensidad de la zona.

## Contenido

Se incluye la función para agregar ruido poisson.
Cuenta tambipen con las funciones:- 
kernelg(sigma): construye un kernel Gaussiano 2D normalizado, con radio 3sigma.
calculo_rmse(imagen, imagen_denoising, mascara=None): calcula RMSE global o por región.

## Experimentos
Se implementó un kernel isotrópico con sigma perteneciente a {0.5, 1.0, 2.0, 3.0, 5.0}, reportandose valores estadísticos.

También se diseñó un kernel adaptativo que estima un mapa de intensidad local a través de un kernel Gaussiano y se aplica el kernel dependiendo de la intensidad local.

## Resultado
s
El kernel isotrópico reduce el ruido pero también difumina los bordes, y el RMSE óptimo depende del σ elegido.

El kernel adaptativo preserva mejor los bordes porque aplica un suavizado más suave en zonas de intensidad baja.

## Pregunta 2 — Difusión anisotrópica

El objetivo es implementar un modelo de difusión anisotropica con diferencias y vecindades. La imagen utilizada para analizar el ruido y denoising es la imagen de Lena con un ruido gaussiano con sigma=0.05 y una semilla fija.

Se implementaron 3 modos de calculo de coeficientes:
 -Variación total
 -Variación total con Laplaciano
 -Exponencial con gradiente y laplaciano

 Además de incluyeron funciones para calculos del mapa c y RMSE.
 
 ### Coeficientes implementados (fórmulas)

| Coeficiente | Fórmula | Parámetros |
|---|---|---|
| **TV clásico** | `c = 1/√(g² + ε²)` | `ε` |
| **TV · Laplaciano** | `c = [1/√(g² + ε²)] · [1/(1 + (ΔI/r)²)]` | `ε`, `r` |
| **Exp grad + lap** | `c = exp(−(g² + f·(ΔI)²) / r²)` | `r`, `f` |

## Experimentos

Se exploró el impacto de los valores de epsilon.
- ε chico: difusión agresiva en zonas planas → staircasing.
- ε grande: difusión suave → se aproxima a un filtrado isotrópico.

Se mostraron tres casos:
- **Evolución lenta:** λ·N muy chico → no filtra.
- **Sobre-suavizado:** λ·N muy grande → borra bordes.
- **Inestabilidad:** λ > ε/4 → el esquema explícito diverge.

Se exploraron los parámetros de las funciones propuestas para el calculo de coeficientes.

Sensibilidad del ruido por el laplaciano
El Laplaciano amplifica el ruido blanco porque responde a cambios de alta frecuencia. Mecanismos de estabilización:
1. Saturación cuadrática del modulador.
2. Parámetro `r` suficientemente grande.
3. Parámetro `f` para controlar el peso del Laplaciano.

Luego se comparan los tres métodos sobre la misma imagen, modificando los parámetros correspondientes, incluyendo el calculo RMSE.

Finalmente se comentan las fortalezas y limitaciones de las funciones calculadoras de coeficientes.





