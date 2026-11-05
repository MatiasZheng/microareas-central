# Reporte de Microáreas · Central

Tablero de NEO Telecom (Central) por microárea (POP), en tres pestañas:

- **Ranking** – ventas, base de clientes y churn por POP, puntaje ponderado y evolución mes a mes.
- **Demanda de OS** – OS por servicio y mes, por tipo de OS, por POP y servicio, POP por mes, POP por dealer y POP por tipo de OS.
- **Cartera por POP** – clientes activos (confirmados por OS / estimados por ciudad, activos / bloqueados), por dealer, y clientes únicos con OS.

Es una sola página (`index.html`), sin servidor: el cruce de datos se hace en el navegador.

> El archivo `.nojekyll` es opcional: si GitHub no lo muestra o no se sube, la página funciona igual.

## Publicar en GitHub Pages

1. Crear un repositorio nuevo en GitHub (por ejemplo `microareas-central`).
2. Subir `index.html`, `.nojekyll` y este `README.md` (botón **Add file → Upload files**).
3. En el repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch**, rama `main`, carpeta `/ (root)` → **Save**.
4. En 1–2 minutos queda en `https://<tu-usuario>.github.io/microareas-central/`.

> ⚠️ En cuentas gratuitas, GitHub Pages solo funciona con repositorios **públicos**: cualquiera con el link vería los números agregados que trae la página (ventas, bajas y base por POP). Los archivos que cargás con el botón **no** se suben a ningún lado; se procesan en tu navegador.

## Archivos

- `index.html` – el tablero que ven todos (sin botones de carga). Lee los datos de `data.json`.
- `data.json` – los datos cruzados. Es lo único que cambia al actualizar.
- `admin.html` – página de carga y publicación (solo para quien actualiza). No está enlazada desde el tablero.

## Actualizar los datos

1. Bajar de ZOY los exports:
   - **Relatorio Suportes** (OS de Central, todos los subtipos)
   - **Relatorio Contratos – activados** (por fecha de activación) → ventas
   - **Relatorio Contratos – cancelados** (por fecha de cancelación) → churn
   - **Relatorio Contratos Infra** (Activos y Bloqueados) → base con POP de la red
   - **Relatorio Contratos** (Activos y Bloqueados, sin filtro de fecha) → dealer *(opcional)*
2. Abrir `https://<usuario>.github.io/<repo>/admin.html` → **Cargar exports** y elegirlos todos juntos.
3. Revisar los números y tocar **Publicar en GitHub**. En 1–2 minutos todos ven los datos nuevos en `index.html`.

La primera vez hay que pegar un **token de GitHub** (se guarda solo en ese navegador): Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token → *Only select repositories* (este repo) → Repository permissions → **Contents: Read and write**.

Sin token: en admin.html usar **Copiar data.json**, abrir `data.json` en el repo → lápiz ✏️ → pegar → **Commit changes**.

## Reglas del cruce

- **Ventas**: activaciones de Dpto. CENTRAL con *TeamVenda* (equipo de venta). Sin equipo = transferencias u otros movimientos, no cuentan.
- **Churn**: cancelados o rescindidos de CENTRAL por fecha *CanceladoEm*. Transferidos no cuentan. Involuntario = motivo "Falta de Pago".
- **Base**: contratos Ativo + Bloqueado de ciudades de Central en ContratoInfra, con el POP que informa la red.
- **POP**: OS de instalación del contrato (ventas) o última OS con POP; si no hay OS, POP dominante de la ciudad (≥85%); si no, SIN POP. POP con menos de 100 OS → OTROS POP.
- **Puntaje del ranking**: participación ponderada en ventas, base y churn (pesos editables).
