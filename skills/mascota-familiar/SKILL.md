---
name: mascota-familiar
description: Diseña sprites de mascota para el plugin claude-familiar (pixel art 16x12) y los guarda en mascotas-familiar.txt en el Escritorio. Úsala cuando el usuario pida crear, dibujar o guardar una mascota/familiar/buddy, o pida ver sus mascotas guardadas.
---

# Mascota familiar

Crea un sprite para `/familiar import` y lo agrega al archivo `mascotas-familiar.txt` en el Escritorio del usuario (en Windows: `[Environment]::GetFolderPath('Desktop')`; en macOS/Linux: `~/Desktop`).

## Formato del sprite (lo exige el plugin)

- JSON `{"palette": {...}, "rows": [...]}`.
- `rows`: exactamente 12 strings de exactamente 16 caracteres.
- `.` es transparente. Cualquier otro carácter debe existir en `palette`.
- Claves de `palette`: un solo carácter, distinto de `.`. Colores: `#rrggbb`.
- La clave `e` es el ojo: se cierra en una línea cuando duerme o se asusta. Úsala para los ojos del personaje principal.
- Solo hay un dibujo fijo; el plugin lo anima solo según el mood (salta, suda, tiembla, brilla, duerme). No existen cuadros de animación propios.
- Si se usa a Clawd, su color es `#d77757`. Forma base:

```
..CCCCCCCCCCCC..
..CCeCCCCCCeCC..
CCCCCCCCCCCCCCCC
..CCCCCCCCCCCC..
...C.C....C.C...
```

Pensar en terminal oscura: evitar colores muy oscuros en el contorno.

## Pasos

1. Diseñar el sprite según lo que pida el usuario.
2. Validarlo antes de mostrarlo. Escribir el JSON en el scratchpad y correr:

```sh
python -c "
import json,sys;s=json.load(open(sys.argv[1]));p=s['palette']
assert len(s['rows'])==12,'deben ser 12 filas'
for k,v in p.items(): assert len(k)==1 and k!='.' and len(v)==7 and v[0]=='#',k
for r in s['rows']: assert len(r)==16 and all(c=='.' or c in p for c in r),r
print('ok')
for r in s['rows']: print(r)" RUTA_DEL_JSON
```

3. Agregar al final de `mascotas-familiar.txt` en el Escritorio (crearlo si no existe; nunca sobrescribir lo que ya tiene) un bloque así:

```
== Nombre de la mascota ==
<las 12 filas, para ver el dibujo>
/familiar import <JSON en una sola línea>

```

4. Responder con el boceto de las filas, el comando `/familiar import ...` listo para pegar y, después, `/familiar save <nombre-corto>` para guardarla en el plugin. El plugin está parcheado localmente con `save`, `wear <nombre>`, `saved` y `forget <nombre>`: con eso se cambia entre mascotas guardadas sin volver a importar.

Si el usuario pide ver sus mascotas, leer el archivo y listar los nombres.
