# Guion de Presentación - Unidad 3: Animaciones en Jetpack Compose

## Introducción (1-2 minutos)

**Hola, hoy veremos la Unidad 3 sobre Animaciones en Jetpack Compose.**

Las animaciones mejoran la experiencia del usuario al:
- Hacer las aplicaciones más interactivas y atractivas
- Facilitar la interpretación de cambios en la interfaz
- Agregar un estilo refinado y profesional

En esta unidad implementamos animaciones en nuestra app de Superhéroes para mostrar información adicional cuando el usuario expande las tarjetas.

---

## 1. Conceptos Fundamentales (2-3 minutos)

### ¿Qué son las animaciones en Compose?

Las animaciones en Jetpack Compose son transiciones suaves entre estados de la interfaz. Jetpack Compose proporciona APIs simples y declarativas para crear animaciones.

### Tipos de Animaciones que Implementamos:

**a) Animaciones de Entrada y Salida (Enter/Exit)**
- `fadeIn` / `fadeOut`: Aparecen/desaparecen elementos
- `slideInVertically`: Elementos que entran desde arriba o abajo
- `AnimatedVisibility`: Controla la visibilidad con animación

**b) Animaciones de Estado**
- `animateContentSize()`: Anima cambios en el tamaño del contenido
- `animateColorAsState()`: Anima cambios de color

**c) Animaciones de Resorte (Spring)**
- Efectos naturales basados en física de resortes
- Parámetros configurables: `dampingRatio` y `stiffness`

---

## 2. Implementación en la App de Superhéroes (3-4 minutos)

### A. Animación de Entrada de la Lista

**Código clave:**
```kotlin
AnimatedVisibility(
    visibleState = visibleState,
    enter = fadeIn(spring(dampingRatio = DampingRatioLowBouncy)),
    exit = fadeOut()
)
```

**¿Qué hace?**
- La lista completa aparece con un efecto de desvanecimiento suave al cargar
- Usa animación de resorte con bajo rebote para un efecto natural

---

### B. Animaciones Escalonadas (Staggered) para Elementos Individuales

**Código clave:**
```kotlin
.animateEnterExit(
    enter = slideInVertically(
        animationSpec = spring(
            stiffness = StiffnessVeryLow,
            dampingRatio = DampingRatioLowBouncy
        ),
        initialOffsetY = { it * (index + 1) }
    )
)
```

**¿Qué hace?**
- Cada tarjeta de superhéroe entra desde abajo con un desfase (staggered)
- El desfase se calcula por el índice del elemento
- Crea un efecto en cascada visualmente atractivo

---

### C. Animación de Expandir/Colapsar en las Tarjetas

**Esta es la funcionalidad principal que implementamos:**

#### 1. Estado Expandible

```kotlin
var expanded by remember { mutableStateOf(false) }
```

- Cada tarjeta tiene su propio estado de expansión independiente
- Se usa `remember` para mantener el estado durante recomposiciones

#### 2. Animación de Tamaño

```kotlin
Column(
    modifier = Modifier.animateContentSize(
        animationSpec = spring(
            dampingRatio = Spring.DampingRatioNoBouncy,
            stiffness = Spring.StiffnessMedium
        )
    )
)
```

**¿Qué hace?**
- Anima el cambio de altura cuando se muestra/oculta la descripción
- `DampingRatioNoBouncy`: Sin rebote, movimiento suave
- `StiffnessMedium`: Resorte con rigidez media para animación equilibrada

#### 3. Animación de Color

```kotlin
val color by animateColorAsState(
    targetValue = if (expanded) 
        MaterialTheme.colorScheme.tertiaryContainer
    else 
        MaterialTheme.colorScheme.primaryContainer
)
```

**¿Qué hace?**
- Cambia suavemente el color de fondo según el estado
- Usa colores del tema Material Design 3
- La transición de color es automática y fluida

#### 4. Botón de Expandir/Colapsar

```kotlin
@Composable
private fun HeroItemButton(
    expanded: Boolean,
    onClick: () -> Unit
) {
    IconButton(onClick = onClick) {
        Icon(
            imageVector = if (expanded) 
                Icons.Filled.ExpandLess 
            else 
                Icons.Filled.ExpandMore
        )
    }
}
```

**¿Qué hace?**
- Muestra ícono `ExpandMore` cuando está colapsada
- Muestra ícono `ExpandLess` cuando está expandida
- Proporciona feedback visual claro al usuario

---

## 3. Parámetros de Animación de Resorte (2 minutos)

### DampingRatio (Relación de Amortiguación)

- **`DampingRatioNoBouncy`**: Sin rebote, movimiento suave y directo
  - Ideal para: Expansión de contenido, cambios de tamaño
  
- **`DampingRatioLowBouncy`**: Poco rebote, efecto ligero
  - Ideal para: Entradas de elementos, efectos lúdicos

### Stiffness (Rigidez)

- **`StiffnessVeryLow`**: Movimiento muy suave y lento
  - Ideal para: Entradas en cascada, efectos dramáticos

- **`StiffnessMedium`**: Equilibrio entre velocidad y suavidad
  - Ideal para: Animaciones de contenido expandible

---

## 4. Flujo de Interacción (1-2 minutos)

**Cuando el usuario toca el botón de expandir:**

1. El estado `expanded` cambia de `false` a `true`
2. `animateColorAsState` detecta el cambio y anima el color de fondo
3. `animateContentSize` detecta que la descripción aparecerá y anima la altura
4. El ícono cambia de `ExpandMore` a `ExpandLess`
5. La descripción aparece con la animación de tamaño

**Resultado:** Una experiencia de usuario fluida y profesional.

---

## 5. Dependencias Necesarias (1 minuto)

Para usar los íconos de Material Design, agregamos:

```kotlin
implementation("androidx.compose.material:material-icons-extended")
```

Esta biblioteca incluye íconos adicionales como `ExpandMore` y `ExpandLess`.

---

## 6. Mejores Prácticas (1-2 minutos)

✅ **Usa animaciones moderadas:**
- No sobrecargues la interfaz con demasiadas animaciones
- Usa animaciones para mejorar la UX, no solo por estética

✅ **Configura parámetros apropiados:**
- Ajusta `dampingRatio` y `stiffness` según el contexto
- Prueba diferentes valores para encontrar el equilibrio

✅ **Mantén consistencia:**
- Usa los mismos parámetros de animación para elementos similares
- Sigue las pautas de Material Design

---

## Conclusión (1 minuto)

**En esta unidad aprendimos:**

1. ✅ Cómo agregar animaciones de entrada para listas completas
2. ✅ Cómo crear efectos escalonados para elementos individuales
3. ✅ Cómo implementar animaciones de expandir/colapsar con `animateContentSize`
4. ✅ Cómo animar cambios de color con `animateColorAsState`
5. ✅ Cómo configurar parámetros de resorte para diferentes efectos

**Las animaciones hacen que nuestras apps sean más profesionales y agradables de usar.**

---

## Preguntas y Respuestas

**Pregunta común:** ¿Las animaciones afectan el rendimiento?

**Respuesta:** Jetpack Compose optimiza las animaciones automáticamente. Sin embargo, evita animaciones complejas en listas muy largas o en elementos que se recomponen frecuentemente.

---

## Recursos Adicionales

- Documentación oficial de Animaciones en Jetpack Compose
- Material Design Motion Guidelines
- Codelab: Animación simple con Jetpack Compose

---

**Tiempo total estimado: 12-15 minutos**

**Con demostración práctica en la app: 15-20 minutos**

