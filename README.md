# Reporte de órdenes de mantenimiento

Este proyecto toma un CSV de órdenes de trabajo ficticias, calcula cuáles están vencidas a una fecha de corte y genera un Excel para consultar el resultado en Power BI Desktop.

Con los datos de ejemplo y corte al **22/11/2026**, el reporte encuentra **4 órdenes válidas, 2 vencidas y 1 registro rechazado**. Las dos vencidas corresponden a la Esmaltadora y acumulan 4 y 2 días de atraso. La fecha inválida queda registrada en la hoja `Errores`.

## Cómo ejecutarlo

Se requiere Python 3.11 o posterior. Los comandos usan PowerShell y se ejecutan desde la raíz del proyecto. Si tu instalación usa el comando `py` en lugar de `python`, reemplazá el primer comando por `py -m venv .venv`.

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe procesar_ordenes.py --entrada datos/ordenes_mantenimiento.csv --salida salida/reporte_mantenimiento.xlsx --fecha-corte 2026-11-22
```

En macOS o Linux, usá `./.venv/bin/python` para ejecutar los dos últimos comandos.

El archivo resultante es `salida/reporte_mantenimiento.xlsx`. Incluye dos hojas:

| Hoja | Contenido |
| --- | --- |
| `Ordenes` | Una fila por orden válida, con su estado calculado y días de atraso. |
| `Errores` | Una fila por registro rechazado, con su número de registro, campo, valor y motivo. |

También podés abrir el [Excel de ejemplo](docs/reporte_mantenimiento_ejemplo.xlsx) para revisar el resultado sin instalar Python.

## Power BI Desktop

Seguí la [guía de conexión y armado del tablero](docs/power-bi.md). Incluye los tipos de datos, las tres medidas DAX y una prueba de actualización. El archivo `.pbix` se crea en esa etapa.

**Demo sugerida.** Cambiá OT-001 a `Completada` en el CSV, volvés a ejecutar el script con el mismo corte y actualizás Power BI. El total de vencidas baja de 2 a 1 y la fecha inválida sigue registrada entre los rechazos.

## Cómo está hecho

- `procesar_ordenes.py` lee, valida y clasifica el CSV, y exporta el Excel.
- `datos/ordenes_mantenimiento.csv` contiene la muestra reproducible.
- [SDD](sdd.md) describe el contrato de datos, las reglas de negocio y los criterios de aceptación.

Para practicar Python, empezá por `clasificar_orden()`. Probá qué devuelve con una fecha anterior, igual y posterior al corte. Después seguí una orden desde el CSV hasta su fila en Excel.
