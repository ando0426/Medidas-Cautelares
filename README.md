# Medidas cautelares 2026: vigentes y levantamientos

Dashboard de seguimiento de medidas cautelares (bloqueos y embargos) y de sus levantamientos (desbloqueos y desembargos). Se publica con GitHub Pages y sus datos se actualizan cada día.

## Cómo está armado

```
index.html                          ← el dashboard (una sola página)
data/
  levantamientos_diarios.csv        ← desbloqueos y desembargos por día
  levantamientos_causal_mes.csv     ← levantamientos por mes y causal
  estado_medidas.csv                ← medidas en cartera según su estado actual
  corte.json                        ← fecha y hora del último corte
```

El dashboard **no tiene cifras escritas dentro**: cada vez que alguien lo abre, lee los archivos de `data/`. Para actualizarlo basta con reemplazar esos CSV. Se pueden abrir y editar en Excel; al guardar, conserva el formato **CSV UTF-8 (delimitado por comas)** y los mismos nombres de columna.

### Columnas

| Archivo | Columnas |
|---|---|
| `levantamientos_diarios.csv` | `dia` (AAAA-MM-DD), `desbloqueos`, `desembargos` |
| `levantamientos_causal_mes.csv` | `mes` (AAAA-MM), `tipo` (Desbloqueo / Desembargo), `causal`, `n` |
| `estado_medidas.csv` | `cod` (B, E, G, D, F, I), `estado`, `medidas`, `valor` |

Los indicadores dependen de estos códigos de estado: `B` bloqueo, `E` embargo, `G` desbloqueo, `D` desembargo.

## Publicarlo en GitHub Pages (Símbolo del sistema)

1. Crea un repositorio vacío en GitHub, por ejemplo `dashboard-medidas`.
2. En la carpeta del proyecto:

```cmd
cd C:\ruta\dashboard-medidas
git init
git add .
git commit -m "Dashboard inicial"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/dashboard-medidas.git
git push -u origin main
```

3. En GitHub: **Settings → Pages → Build and deployment → Source: Deploy from a branch → main / (root) → Save**.
4. En 1 o 2 minutos queda en `https://TU_USUARIO.github.io/dashboard-medidas/`.

## Verlo en tu PC antes de publicar

Al abrir `index.html` con doble clic el navegador bloquea la lectura de los CSV. Sírvelo así:

```cmd
cd C:\ruta\dashboard-medidas
python -m http.server 8000
```

y abre `http://localhost:8000`.

## Actualización diaria

Una tarea programada de Claude consulta Oracle GENESYS cada mañana, regenera los tres CSV y `corte.json`, y hace commit a este repositorio. GitHub Pages vuelve a publicar solo.
