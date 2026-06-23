# Generación de Gráficos - Instrucciones para la IA

## Formato de Salida

Para generar un gráfico, debes incluir un bloque de código con lenguaje `chart` en tu respuesta markdown:

```markdown
Texto explicativo antes del gráfico...

```chart
{
  "title": "Título del gráfico",
  "x": {
    "values": ["Label1", "Label2", "Label3"]
  },
  "series": [
    {
      "type": "bar",
      "label": "Nombre de la serie",
      "data": [100, 200, 150]
    }
  ]
}
```

Texto opcional después del gráfico...
```

Puedes incluir múltiples bloques ` ```chart ` en una misma respuesta si necesitas varios gráficos.

## Estructura JSON

### Campos obligatorios

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `title` | `string` | Título visible del gráfico |
| `x.values` | `string[]` | Etiquetas del eje X (categorías) |
| `series` | `array` | Arreglo de series de datos (mínimo 1) |

### Serie de datos

Cada serie tiene:

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `type` | `string` | Tipo de gráfico (ver más abajo) |
| `label` | `string` | Nombre de la serie (aparece en la leyenda) |
| `data` | `number[]` | Valores numéricos (misma longitud que `x.values`) |

### Tipos de gráfico soportados

| Tipo | Descripción |
|------|-------------|
| `bar` | Barras verticales (por defecto) |
| `line` | Línea continua |
| `area` | Área rellena |
| `donut` / `dona` / `doughnut` / `pie` | Gráfico de donut (torta) |

## Reglas importantes

1. **Longitud de datos**: `x.values` y `series[].data` deben tener la misma longitud
2. **Múltiples series**: puedes incluir varias series con diferentes `type` en un mismo gráfico (ej: barras + línea)
3. **Donut**: si una serie es tipo `donut`, solo se usará la primera serie de ese tipo; las demás se ignoran
4. **Valores**: todos los valores en `data` deben ser números finitos (no `null`, `undefined`, `NaN` o `Infinity`)
5. **Sin color**: no incluyas `color` en las series; el frontend asigna colores automáticamente desde su paleta
6. **Título**: siempre debe ser un string no vacío

## Ejemplos

### Barra simple

```chart
{
  "title": "Productos por categoría",
  "x": { "values": ["Electrónica", "Hogar", "Deportes", "Ropa"] },
  "series": [
    { "type": "bar", "label": "Cantidad", "data": [45, 120, 67, 89] }
  ]
}
```

### Línea

```chart
{
  "title": "Evolución de ventas mensuales",
  "x": { "values": ["Ene", "Feb", "Mar", "Abr", "May", "Jun"] },
  "series": [
    { "type": "line", "label": "Ventas 2024", "data": [12000, 15000, 13500, 17000, 16000, 19000] }
  ]
}
```

### Área

```chart
{
  "title": "Usuarios activos",
  "x": { "values": ["Q1", "Q2", "Q3", "Q4"] },
  "series": [
    { "type": "area", "label": "Usuarios", "data": [5000, 7800, 9200, 11500] }
  ]
}
```

### Donut

```chart
{
  "title": "Distribución de presupuesto",
  "x": { "values": ["Marketing", "Desarrollo", "Operaciones", "RRHH"] },
  "series": [
    { "type": "donut", "label": "Porcentaje", "data": [35, 30, 20, 15] }
  ]
}
```

### Múltiples series (barras + línea)

```chart
{
  "title": "Ingresos vs Gastos",
  "x": { "values": ["Ene", "Feb", "Mar", "Abr"] },
  "series": [
    { "type": "bar", "label": "Ingresos", "data": [50000, 55000, 48000, 60000] },
    { "type": "line", "label": "Gastos", "data": [30000, 32000, 31000, 35000] }
  ]
}
```

## Precauciones

- Cuando los datos estén llegando vía streaming, el frontend mostrará un skeleton ("Generando gráfico...") hasta que el JSON esté completo y sea válido
- Si el JSON es inválido (parse error, campos faltantes, tipos incorrectos), el frontend mostrará un error con el código JSON visible para depuración
- Para conjuntos de datos grandes con muchas categorías en el eje X, el gráfico se vuelve horizontalmente desplazable
- Siempre genera el bloque ` ```chart ` **después** de haber explicado textualmente los datos, no como reemplazo del texto
