# Microáreas Central · Ventas, Base y Churn

Tablero de NEO Telecom (Central) que rankea las microáreas (POP) según **ventas**, **base de clientes** y **churn**, con evolución mes a mes.

Es una sola página (`index.html`), sin servidor: el cruce de datos se hace en el navegador.

## Publicar en GitHub Pages

1. Crear un repositorio nuevo en GitHub (por ejemplo `microareas-central`).
2. Subir `index.html`, `.nojekyll` y este `README.md` (botón **Add file → Upload files**).
3. En el repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, rama `main`, carpeta `/ (root)` → **Save**.
4. En 1–2 minutos queda en `https://<tu-usuario>.github.io/microareas-central/`.

> ⚠️ En cuentas gratuitas, GitHub Pages solo funciona con repositorios **públicos**: cualquiera con el link vería los números agregados que trae la página (ventas, bajas y base por POP). Los archivos que cargás con el botón **no** se suben a ningún lado; se procesan en tu navegador.

## Actualizar los datos (cruce)

1. Bajar de ZOY los 4 exports:
   - **Relatorio Suportes** (OS de Central, todos los subtipos) → da el POP de cada contrato.
   - **Relatorio Contratos – activados** (por fecha de activación) → ventas.
   - **Relatorio Contratos – cancelados** (por fecha de cancelación) → churn.
   - **Relatorio Contratos Carteiras** → base activa.
2. Abrir la página → **Cargar los 4 exports** (o arrastrarlos sobre la banda violeta) y elegir los 4 juntos.
3. La página detecta cada archivo por sus columnas, cruza y redibuja todo. Queda guardado en ese navegador.
4. **Descargar cruce (.xlsx)** baja el detalle con el POP asignado (hojas Ventas, Churn y Base Clientes).

Para que **todos** vean los datos nuevos (no solo tu navegador), pedí que se regenere `index.html` con los datos actuales y volvé a subirlo al repo.

## Reglas del cruce

- **Ventas**: activaciones de Dpto. CENTRAL con *TeamVenda* (equipo de venta). Sin equipo = transferencias u otros movimientos, no cuentan.
- **Churn**: cancelados o rescindidos de CENTRAL por fecha *CanceladoEm*. Transferidos no cuentan. Involuntario = motivo "Falta de Pago".
- **Base**: contratos Ativo + Bloqueado de ciudades de Central en la cartera.
- **POP**: OS de instalación del contrato (ventas) o última OS con POP; si no hay OS, POP dominante de la ciudad (≥85%); si no, SIN POP. POP con menos de 100 OS → OTROS POP.
- **Puntaje del ranking**: participación ponderada en ventas, base y churn (pesos editables).
