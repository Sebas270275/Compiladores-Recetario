# RecetaLang

RecetaLang es un lenguaje de dominio específico para representar recetas mediante instrucciones de ingredientes, mezcla y cocción. Este repositorio contiene un compilador pequeño en Python, escrito a mano para facilitar su lectura y explicación en la defensa del curso de Compiladores.

## Requisitos

- Python 3.10 o posterior.
- Solo biblioteca estándar de Python; no requiere instalación de paquetes.
- Los programas se guardan en archivos UTF-8 con extensión `.receta`.

## Entrega de Actividad 3: tres buenos y tres malos

La carpeta `ejemplos/actividad3/` contiene exactamente seis programas `.receta`: tres válidos y tres con errores intencionales. Sus salidas y mensajes están en [`ejemplos/actividad3/ESPERADO.md`](ejemplos/actividad3/ESPERADO.md).

| Grupo | Programa | Qué demuestra |
|---|---|---|
| Bueno 1 | `01_bueno_preparacion_basica.receta` | Variables, agregar ingredientes, mezclar y cocinar |
| Bueno 2 | `02_bueno_cantidades_calculadas.receta` | Cálculos con `+`, `-` y `*` |
| Bueno 3 | `03_bueno_repeticiones_y_decisiones.receta` | Condiciones, comparadores y repetición |
| Malo 1 | `04_malo_error_lexico.receta` | Símbolo `@` no reconocido |
| Malo 2 | `05_malo_error_sintactico.receta` | Falta el `fin` de un bloque |
| Malo 3 | `06_malo_error_semantico.receta` | Variable usada antes de definirse |

Los otros ejemplos de `ejemplos/` y `ejemplos/extra/` son casos adicionales de verificación del compilador; no forman parte de ese conjunto de seis.

## Ejecutar una receta

Desde la carpeta del proyecto:

```powershell
python -m recetalang ejemplos/01_preparacion_basica.receta
```

Para ejecutar uno de los tres programas buenos de la entrega:

```powershell
python -m recetalang ejemplos/actividad3/01_bueno_preparacion_basica.receta
```

Los tres programas malos también se ejecutan con ese comando; terminan con código 1 y muestran el error correspondiente. La CLI compila y ejecuta por defecto. El proceso convierte el texto en tokens, crea un AST, valida variables y reglas del recetario, genera bytecode y lo ejecuta en una máquina virtual. También permite inspeccionar cada etapa:

```powershell
python -m recetalang ejemplos/03_repeticiones_y_decisiones.receta --tokens
python -m recetalang ejemplos/03_repeticiones_y_decisiones.receta --ast
python -m recetalang ejemplos/03_repeticiones_y_decisiones.receta --simbolos
python -m recetalang ejemplos/03_repeticiones_y_decisiones.receta --bytecode
python -m recetalang ejemplos/03_repeticiones_y_decisiones.receta --solo-compilar
```

Los errores muestran la fase y la posición en formato `Línea L, columna C: mensaje`. El código de salida es `0` si la compilación termina correctamente y `1` si ocurre un error.

## Ejemplo de salida

Para `ejemplos/01_preparacion_basica.receta`:

```text
AGREGAR(ingrediente=harina, cantidad=200, unidad=g)
AGREGAR(ingrediente=leche, cantidad=150, unidad=ml)
MEZCLAR(minutos=3)
COCINAR(minutos=25)
```

## Pruebas y reportes

```powershell
python -m unittest discover
python pruebas/ejecutar_pruebas.py
```

El primer comando corre las pruebas automatizadas. El segundo genera `resultados/resultados_actividad5.txt` y `resultados/resultados_final.txt`; el reporte final procesa los nueve ejemplos principales y los cinco casos adicionales.

## Estructura

- `recetalang/`: lexer, parser, AST, análisis semántico, generador, VM y CLI.
- `gramatica/recetalang.ebnf`: gramática formal.
- `ejemplos/`: programas base, casos adicionales y `ESPERADO.md`.
- `ejemplos/actividad3/`: conjunto de entrega de seis programas: tres correctos y tres con errores.
- `pruebas/`: pruebas `unittest` y ejecutor de reportes.
- `resultados/`: reportes reproducibles de las actividades.
- `docs/`: especificación, fases, guías, defensa y plan de commits.
- `tools/empaquetar.py`: genera paquetes por fase y el zip final.
- `entregas/`: archivos zip creados; no se incluyen dentro de otros paquetes.

El flujo interno es: texto → lexer → parser y AST → análisis semántico → bytecode → máquina virtual → registro de la receta.
