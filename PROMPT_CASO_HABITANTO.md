# Prompt para Claude Code — Caso de estudio Habitanto

Copia y pega esto en Claude Code (dentro de esta carpeta) para construir o continuar el caso de estudio de Habitanto con el mismo criterio que el resto del portafolio.

---

Lee primero `CLAUDE.md` en la raíz de este repo para entender la arquitectura del sitio, el patrón de páginas de case study y las reglas de contenido. Después, trabaja sobre `habitanto.html` con este contexto:

## Qué es Habitanto

Habitanto administra condominios. Trabajé ahí en el rediseño de dos apps (App Residente y App Administrador), el sitio web institucional, y varios proyectos de optimización del sistema de pagos dentro de la plataforma.

- **App Residente:** comunicación con el condominio, seguimiento de pagos y tareas, reservas de áreas comunes, marketplace de la comunidad, invitaciones de visitas con QR.
- **App Administrador:** cobranza y morosidad, confirmación de reservas, gestión de contactos, novedades/incidencias, reportes.
- **App Guardia:** una tercera app para el personal de seguridad (tareas, registro de paquetes, emergencias).
- **Panel Web:** versión de escritorio para el administrador (cobranza, reportes, novedades).
- **Sitio web institucional.**

El proyecto más profundo, y el que sostiene la narrativa principal de `habitanto.html`, es el **sistema de pagos**: consolidar el checkout del residente, un marketplace de servicios del hogar (agua, luz, gas, etc.), activación de pago automático con dos modelos de incentivo, y las herramientas de cobranza del administrador.

## Fuente del diseño

Los Figma son de solo vista (sesión bloqueada, sin login, herramientas de interacción deshabilitadas):

- App Residente: `https://www.figma.com/design/pZsC4e1YYSmibGRgo6Cc1k/New-app-residente`
- App Administrador: `https://www.figma.com/design/KPVMgMOPfYbwvBwwtyRTVu/App-Administrador-Habitanto`

Para extraer capturas reales de estos archivos (Claude Code no tiene acceso directo a Figma, esto se hizo con Claude en modo Cowork usando el navegador):
1. Navegar al `node-id` del frame específico (`?node-id=X-Y&zoom=1`) para que Figma haga auto-fit.
2. Si el canvas está en modo "mano" bloqueado, usar el panel de Layers: hacer clic en el nombre de una capa selecciona el frame y actualiza el `node-id` en la URL — luego renavegar a esa URL fresca para que haga zoom automático.
3. Usar zoom con `region` + `save_to_disk: true` para capturar y guardar el recorte exacto como archivo.
4. Copiar el archivo a `assets/habitanto/` con un nombre descriptivo en kebab-case (`residente-…`, `admin-…`, `proceso-…`).

**Nunca inventar una captura.** Si una funcionalidad no tiene una captura real disponible, descríbela en prosa (ver la lista "Fuera de esta página" al final de `habitanto.html`) en vez de apuntar un `<img>` a un archivo que no existe.

## Reglas de contenido (confirmadas por Cindy)

- El modal post-pago para activar el pago automático (`residente-modal-intento2.png`) fue una **iteración posterior** a los banners (`residente-banners-intento1.png`) — por eso el banner es el intento ❌ y el modal es el ✅.
- **No hay métricas duras confirmadas** (no hay % de reducción de mora, ni días de cobro exactos). No inventar números — describir el impacto de forma cualitativa.
- Dos modelos de incentivo reales para el pago automático: en algunos edificios el residente asume la comisión del medio de pago y activar el automático baja la tarifa (4,50% → 4,05%); en otros el edificio absorbe la comisión y el incentivo son millas acumulables.
- La permanencia mínima del pago automático es de 12 meses — es una regla de negocio, no una decisión de diseño; explicarla con fecha exacta en vez de solo bloquear el botón.

## Estructura que debe tener la página

Sigue el patrón de case study ya descrito en `CLAUDE.md` (navbar → hero → strip nav numerada → secciones alternadas `.attempt`/`.attempt.reverse` → pull quote → CTA). Dentro de eso, `habitanto.html` organiza el contenido en piezas:

1. **El contexto** — cobrar dependía de perseguir a alguien, de los dos lados (residente y administrador).
2. **El proceso** — capturas del mapeo de flujos en Figma (etapa temprana del proyecto, antes del pulido visual — aclarar esto en el caption, no ocultarlo).
3. **El checkout** — de pagos separados a un solo checkout consolidado (Habipay).
4. **Servicios** — marketplace de servicios del hogar dentro de la misma app.
5. **Activación del pago automático** — intento 1 (banners, ❌) vs. intento 2 (modal post-pago, ✅), y la regla de los 12 meses explicada con fecha.
6. **El otro lado (administrador)** — dashboard de morosidad, link de pago con mensaje bilingüe automático, reportes por unidad, conciliación manual.
7. **Toda la plataforma** — todo lo que queda fuera del sistema de pagos pero fue parte del rediseño: reservas (residente + confirmación del admin), comunidad/marketplace de anuncios, invitaciones con QR, perfil, reportar pago manual, contactos del admin. Cierra con una lista breve y honesta ("Fuera de esta página") mencionando App Guardia, Panel Web y el sitio institucional — sin capturas si no se lograron extraer limpias, mejor en texto que forzar una imagen de mala calidad o inventada.

## Después de construir o editar

- Verificar la página completa en el navegador (`python3 -m http.server 8765` desde la raíz del repo, abrir `http://localhost:8765/habitanto.html`, hacer scroll de punta a punta) — no hay test automatizado, la única verificación es visual.
- Confirmar que `work.html` y `El Ojo.dc.html` sigan apuntando a `./habitanto.html` como el caso completo (no a un ancla placeholder).
