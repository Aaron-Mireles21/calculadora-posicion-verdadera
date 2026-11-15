# Calculadora de posición verdadera

Calculadora web de **posición verdadera (True Position, GD&T)** para una característica en el plano XY y para un **barreno inclinado en 3D**. Muestra el cálculo paso a paso, dice si la pieza cumple y dibuja la zona de tolerancia con el punto medido.

Es una sola página, sin instalación y sin dependencias: se abre en el navegador.

<!-- Agrega aquí una captura de pantalla: ![Captura de la calculadora](docs/captura.png) -->

## El problema

La posición verdadera es un cálculo que se repite todo el tiempo en inspección dimensional, y en una hoja de cálculo es fácil equivocarse de fórmula o perder de vista qué paso dio qué número. En barrenos inclinados es más fácil todavía: parte del error de la medición es de profundidad, va a lo largo del eje y **no cuenta** para la posición.

Esta herramienta hace el cálculo completo y deja cada paso a la vista para poder revisarlo.

## Qué hace

**Posición 2D**
- Recibe la posición nominal y la medida en X y Y, y la tolerancia de posición Ø.
- Calcula las desviaciones y la posición verdadera.
- Compara contra la tolerancia y muestra cuánto de la zona se usa.

**Barreno inclinado 3D**
- Recibe la posición nominal y la medida en X, Y y Z, y el ángulo de inclinación θ del eje respecto a Z.
- Calcula el vector unitario del eje, separa el error axial (profundidad) del error perpendicular y obtiene la posición verdadera solo con la parte perpendicular.
- Muestra los cinco pasos del cálculo.

En ambos modos se dibuja la zona de tolerancia y la posición del punto medido.

## Método

### 2D

```
ΔX = X medido − X nominal
ΔY = Y medido − Y nominal
TP = 2 · √(ΔX² + ΔY²)
```

### Barreno inclinado en 3D

El eje se inclina en el plano XZ, con el ángulo θ medido desde Z.

```
Paso 1  ΔX, ΔY, ΔZ = medido − nominal
Paso 2  I = sin θ    J = 0    K = cos θ
Paso 3  d = ΔX·I + ΔY·J + ΔZ·K            (error axial, de profundidad)
Paso 4  ΔX⊥ = ΔX − d·I
        ΔY⊥ = ΔY − d·J
        ΔZ⊥ = ΔZ − d·K
Paso 5  TP = 2 · √(ΔX⊥² + ΔY⊥² + ΔZ⊥²)
```

La pieza cumple si `TP ≤ tolerancia`. Un desplazamiento de 0.100 mm solo en Z, con un eje a 18°, aporta aproximadamente `0.100 · sin 18° ≈ 0.031` mm al radio de la zona.

El método sigue la proyección vectorial para ejes inclinados usada con ASME Y14.5.

## Ejemplos incluidos

Los valores que trae la página al abrirla son inventados.

| Caso | Datos | Resultado |
| --- | --- | --- |
| 2D | Nominal (25.000, 40.000), medido (25.018, 39.985), tolerancia 0.080 | TP = 0.047, cumple |
| 3D | θ = 18°, nominal (0, 0, 0), medido (0.020, 0.010, −0.100), tolerancia 0.080 | d = −0.089, TP = 0.102, no cumple |

## Cómo usarla

Abre la versión publicada: **[ENLACE A GITHUB PAGES]**

O úsala en tu computadora: descarga `index.html` y ábrelo en cualquier navegador. No necesita servidor ni instalación.

Las fuentes se cargan desde Google Fonts. Sin conexión, el navegador usa fuentes del sistema y todo sigue funcionando.

## Tecnologías

HTML, CSS y JavaScript sin librerías. La gráfica de la zona de tolerancia es un SVG generado por el propio script.

## Estructura

```
index.html    página completa: estilos, cálculo y gráfica
README.md     este archivo
```

## Aviso

Es una herramienta educativa que se usa con datos de ejemplo y es independiente de cualquier empresa o cliente. Antes de usarla para aceptar o rechazar piezas, compara el resultado con tu propio procedimiento y con la edición vigente de ASME Y14.5.

## Autor

Aaron Mireles · [GitHub](https://github.com/Aaron-Mireles21) · [YouTube](https://www.youtube.com/@mirelesaaron)
