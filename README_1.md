# Gestión de Siniestros — Puesta en marcha

## 1. Crear el Google Sheet + Apps Script
1. Andá a Google Sheets y creá una planilla nueva (ej: "Siniestros - Base de datos").
2. Extensiones → Apps Script.
3. Borrá el contenido del editor y pegá todo el contenido de `Code.gs`.
4. Guardá (ícono disquete).
5. Implementar → Nueva implementación → tipo **Aplicación web**.
   - Ejecutar como: **Yo**
   - Quién tiene acceso: **Cualquier usuario**
6. Copiá la URL que termina en `/exec`.
7. La primera vez que alguien entre (doGet/doPost), el script crea solo las hojas
   `Usuarios`, `Siniestros`, `Etapas`, `HistorialEtapas` y un usuario admin de arranque:
   - usuario: `admin`
   - contraseña: `admin2026`
   - PIN: `2026`
   **Cambialo apenas entres** (Admin → Usuarios, o directo en la hoja `Usuarios`).

## 2. Configurar el HTML
Abrí `siniestros_dashboard.html` y en la parte de arriba del `<script>` pegá la URL:

```js
const CONFIG = {
  APPS_SCRIPT_URL: 'https://script.google.com/macros/s/XXXXX/exec',
  ...
};
```

## 3. Subir a GitHub (opcional, igual que el taller)
```bash
git init
git add .
git commit -m "Gestión de Siniestros - v1"
git remote add origin <tu-repo>
git push -u origin main
```
Podés servir el HTML desde GitHub Pages o simplemente abrirlo local / subirlo a tu hosting.

## 4. Uso
- **Nuevo siniestro**: cualquier asesor carga Cliente, Dominio, Modelo, Compañía de
  seguro, Perito, N° de siniestro y Detalle. Queda asignado automáticamente a quien
  lo cargó.
- **Trackeo**: buscador por cliente/dominio/N° de siniestro. Al seleccionar un caso
  se ve la hoja de ruta visual (Orden de trabajo → Presupuesto → Orden autorizada
  compañía → Pedido de piezas → Reparación → Finalizado → Entregado). Tocando una
  etapa se mueve el caso a esa instancia.
- **Mis casos / Todos los casos**: los asesores solo ven sus propios siniestros; el
  admin ve todos. Se puede editar cualquier campo desde ahí.
- **Administrador** (protegido con PIN de 4 dígitos):
  - Crear usuarios por sucursal (DQ / Sabattini), con usuario + contraseña, y rol
    asesor o admin.
  - Editar, agregar, reordenar o eliminar las etapas del proceso — el listado que
    se usa en el Trackeo se actualiza automáticamente.

## 5. Notas técnicas
- Todo corre en un único archivo HTML, sin dependencias externas (mismo criterio
  que `taller_dashboard`).
- Sincroniza con el Sheet cada 15 segundos y además al hacer cualquier acción.
- Guarda una copia local (localStorage) para que la sesión no se pierda al
  recargar.
- La autenticación es simple (usuario/contraseña en texto plano en la hoja
  `Usuarios`) — pensada para uso interno de la concesionaria, no expone
  contraseñas ni PIN al frontend en las consultas de listado.
- Cuando el volumen de siniestros crezca mucho, se puede aplicar la misma
  estrategia que en `taller_dashboard` (tabs dedicados, sync con merge, etc.).
  Por ahora con una sola hoja `Siniestros` alcanza sobra.

## Pendiente / próximos pasos posibles
- Notificación por WhatsApp/email al cambiar de etapa.
- Adjuntar fotos/documentos por siniestro (Google Drive).
- Reportes de tiempos promedio por etapa.
