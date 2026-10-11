# PROJECT.md — JCVH | Plataforma web

> **Estado:** Borrador funcional v0.4 — repositorio de destino seleccionado; pendiente de autorización para publicación e implementación.  
> **Destino:** misión de desarrollo de Orchestor, no anteproyecto académico.  
> **Idioma principal:** español.  
> **Organización de referencia:** Jóvenes Contra la Violencia Honduras (JCVH). El prototipo no implica afiliación institucional oficial ni autorización para utilizar marcas o datos reales.  
> **Repositorio de destino:** https://github.com/Osarii/test_1 (público, vacío al comprobarlo el 11 de octubre de 2026; sin contenido publicado todavía por Orchestor).

## 1. Objetivo del producto

Construir una **plataforma web demostrable, accesible y responsiva** para presentar iniciativas de prevención de la violencia y del desplazamiento forzado, recursos informativos y sedes/regiones de atención de JCVH. Debe incluir un espacio administrativo con CRUD y métricas, autenticación de demostración, un asistente informativo de IA y dos flujos de n8n.

**Resultado esperado:** una aplicación navegable y funcional, no un anteproyecto, un PDF de requisitos ni pantallas estáticas sin interacción.

## 2. Contexto y cobertura

- Público de referencia: jóvenes, comunidad general, organizaciones colaboradoras y personal encargado de la administración de contenidos.
- Sedes o ciudades indicadas para organizar la información del prototipo: **Tegucigalpa, La Ceiba, Comayagua, El Progreso, San Pedro Sula y Siguatepeque**.
- No inventar direcciones, teléfonos, horarios, servicios o estadísticas oficiales de JCVH. Usar contenido demostrativo **claramente marcado como ejemplo** hasta disponer de información verificada.
- **Dirección visual abierta:** Orchestor y sus agentes decidirán identidad visual, paleta, tipografías, composición, diseño de componentes, navegación y tratamiento editorial. No existe una propuesta gráfica prescrita por el usuario. Las decisiones se justificarán brevemente y se evaluarán por calidad, accesibilidad y coherencia con el propósito de JCVH.

## 3. Alcance funcional mínimo (MVP)

### 3.1 Sección pública

1. **Inicio:** propósito del proyecto, iniciativas destacadas, regiones, acceso a recursos y llamados a la acción.
2. **Iniciativas:** catálogo con búsqueda, filtros por tema/ciudad y fichas con información editorial.
3. **Sedes y cobertura:** listado por las seis ciudades anteriores; mapa opcional con datos verificados, sin geolocalización personal obligatoria.
4. **Recursos:** publicaciones y guías informativas sobre prevención y apoyo comunitario, con fuentes visibles.
5. **Contacto institucional:** información verificable o contenido de demostración; no ofrecer un formulario de denuncia confidencial como si fuese un servicio real.
6. **Acceso:** inicio de sesión y registro **solo de cuentas de demostración**.

### 3.2 Área autenticada y administrativa

- **Usuario registrado:** consultar contenido y gestionar una cuenta de prueba sin datos sensibles.
- **Administrador:** CRUD completo de iniciativas, recursos y sedes de demostración; gestionar estados de publicación y cuentas simuladas; acceder al dashboard.
- **Rutas privadas y control de roles:** impedir acceso administrativo desde cuentas comunes, tanto en navegación como en la capa de servicios simulada. No presentar seguridad de JSON Server como autenticación de producción.
- **Dashboard:** contadores, un gráfico y filtros sobre **datos operativos ficticios** (por ejemplo, iniciativas publicadas, recursos, actividad editorial y ciudades cubiertas). No inventar estadísticas reales de víctimas o desplazamiento.

### 3.3 IA y automatización

- **Asistente informativo:** responder preguntas generales sobre navegación, iniciativas, recursos publicados y conceptos educativos. Debe distinguir contenido verificado de demostración; no proporcionar asesoría psicológica, jurídica, ni evaluar riesgo personal.
- **Flujo n8n A — Consulta informativa:** webhook de una consulta general → validación y límite de tamaño → consulta de contenido público aprobado / proveedor de IA autorizado → respuesta con fuentes o salida de demostración. Sin registrar conversaciones personales ni datos de posibles víctimas.
- **Flujo n8n B — Publicación editorial:** cambio de estado de una iniciativa simulada → validación de evento → generación de resumen/notificación administrativa de demostración → registro de resultado e idempotencia. Sin envíos externos reales hasta autorizar destino y credenciales.
- Entregar **dos exportaciones JSON importables de n8n**, documentación mínima de variables de entorno y pruebas con payloads sintéticos. Nunca incrustar credenciales.

## 4. Pila y arquitectura

**Requisitos académicos ya establecidos:** React, React Router DOM, JSON Server con `db.json`, carpeta `services`, servicios locales y externos, diseño responsive, accesibilidad, login/registro, roles, rutas privadas, CRUD, dashboard con métrica y gráfico, Jest, IA y dos flujos n8n.

**Decisiones propuestas para el MVP (ajustables tras inspeccionar un repositorio existente):**

- React + Vite, componentes reutilizables y separación por rutas, componentes, servicios y datos.
- React Router DOM para rutas públicas y protegidas.
- `json-server` / `db.json` para datos **exclusivamente sintéticos**; las credenciales de demostración no deben considerarse seguras para producción.
- Servicios `src/services/local/*` para persistencia simulada y `src/services/external/*` para adaptadores externos. Si un servicio externo requiere claves, ofrecer una alternativa **explícitamente simulada**, no resultados inventados como reales.
- Jest + React Testing Library para pruebas de UI, lógica y permisos; pruebas de integración de servicios y flujos n8n.
- Variables de entorno para URLs y secretos de servidores. Nunca poner secretos en el frontend, los logs ni GitHub.
- Español por defecto; estructura preparada para internacionalización. Habilitar otro idioma únicamente si su contenido puede mantenerse fiel y completo.

## 5. Datos iniciales (solo sintéticos)

Crear colecciones demostrativas en `db.json`:

- `initiatives`: `id`, `title`, `summary`, `city`, `theme`, `status`, `createdAt`, `updatedAt`.
- `resources`: `id`, `title`, `category`, `summary`, `sourceUrl` (si está verificada), `status`.
- `centers`: `id`, `city`, `displayName`, `description`, `verified` (sin inventar dirección/servicios).
- `users`: cuentas simuladas para roles y pruebas; nunca representar el esquema como apto para identidad real.
- `editorialEvents`: eventos administrativos sintéticos para el dashboard y n8n.

Usar identificadores estables, validación de entradas, manejo de estados vacíos, carga y error, y ningún contenido personal real.

## 6. Seguridad, privacidad y límites estrictos

1. **No recolectar ni almacenar** nombres, direcciones, ubicación precisa, testimonios, reportes de violencia ni datos de personas desplazadas o menores.
2. No crear expedientes de víctimas, sistemas reales de denuncia o evaluación individual de riesgo dentro de este MVP.
3. Mostrar una indicación clara de **prototipo educativo / datos demostrativos** en funciones simuladas.
4. El asistente no debe solicitar datos personales ni aparentar actuar como línea de emergencia. Para situaciones urgentes, remitir de forma genérica a servicios de emergencia o canales oficiales verificados, sin inventar contactos.
5. Los formularios se validan y limitan; no transmitir consultas privadas a proveedores externos por defecto.
6. Prohibido incluir claves, cookies, tokens y archivos de sesión en commits, evidencias o prompts.
7. Revisión obligatoria de cualquier integración con proveedores externos antes de habilitarla en producción.

## 7. Autonomía de diseño, experiencia de usuario y accesibilidad

**Instrucción del usuario:** «Dejemos que Orchestor elija el estilo y todo; quiero ver cómo funciona sin darle muchas indicaciones visuales».

- **Libertad creativa:** Orchestor propone y selecciona la identidad visual inicial sin consultar por una paleta, fuente, hero, animación, distribución de secciones o plantilla específicos. Puede optar por una solución editorial, comunitaria, contemporánea o cualquier otra dirección que justifique en función del producto.
- **Decisiones justificadas:** registrar brevemente en `DESIGN_DECISIONS.md` qué dirección eligió, por qué, cómo se refleja en los componentes y qué referencias investigó. No pedir aprobación de cada detalle estético durante la primera propuesta; presentar el resultado para revisión posterior del usuario.
- **No copiar las referencias:** enlaces y fotografía son **referencias de contenido/contexto comunitario**, no instrucciones de reproducir colores, composición, tipografía, logos o diseño de Facebook.
- **Libertad dentro de los requisitos:** el equipo puede proponer el sistema de diseño, estructura de navegación, composición de páginas y UX, pero debe conservar las funcionalidades, tecnologías académicas, privacidad y demás criterios verificables del documento.
- **Calidad mínima no negociable:** diseño responsive en móvil, tableta y escritorio; navegación por teclado, foco visible, etiquetas de formularios, estados de carga/error/vacío, contraste objetivo WCAG 2.2 AA y respeto por movimiento reducido.
- **Criterio ético:** evitar representaciones sensacionalistas de la violencia. No usar fotografías identificables de personas vulnerables sin autorización expresa de publicación.

## 8. Criterios de aceptación

1. Todas las rutas públicas cargan y funcionan en móvil y escritorio.
2. La lista de iniciativas permite consultar, buscar y filtrar; solo el rol autorizado puede realizar CRUD.
3. Registro/login de demostración, rutas privadas y permisos de rol pasan pruebas positivas y negativas.
4. Dashboard presenta al menos un gráfico y métricas derivadas de `db.json` sin datos reales de víctimas.
5. La carpeta `services` contiene al menos un servicio local y un adaptador externo con errores y fallback explícitos.
6. El asistente contesta preguntas generales con límites de privacidad; sin configuración de IA, muestra estado demo claramente identificado.
7. Dos flujos n8n importables se validan con payloads sintéticos sin credenciales ni efectos externos no autorizados.
8. `Jest` pasa las pruebas de componentes, CRUD, autenticación de demostración y servicios.
9. El build de producción y las comprobaciones de accesibilidad y navegación principales pasan.
10. README con instalación, comandos de desarrollo/pruebas, cuentas de prueba, servicios y limitaciones del prototipo.

## 9. Fases sugeridas para que Orchestor planifique

| Fase | Objetivo | Dependencias |
|---|---|---|
| F0 | Inicializar repositorio, reglas de agentes, datos sintéticos y pruebas base | Ninguna |
| F1 | Orchestor elige dirección visual, registra decisiones, implementa layout, rutas públicas y componentes accesibles | F0 |
| F2 | Servicios y CRUD editorial; sedes, iniciativas y recursos | F0–F1 |
| F3 | Autenticación de demostración, roles, rutas privadas y dashboard | F2 |
| F4 | Asistente informativo y dos flujos n8n | F2–F3 |
| F5 | QA, seguridad de demo, accesibilidad, documentación y entrega | F1–F4 |

Las fases son una **propuesta de planificación**, no instrucciones para ejecutar modelos inmediatamente. Orchestor deberá convertirlas en tareas pequeñas con dueños, dependencias, archivos autorizados y criterios comprobables.

## 10. Contrato de ejecución para Orchestor

- **Director:** ChatGPT en esta conversación. GitHub es el transporte de solicitudes/revisiones `ra`.
- **Agentes de desarrollo:** Claude Code y Codex, cada uno en un worktree/rama independiente; el Scheduler selecciona según el plan validado.
- **Modelos:** fijar explícitamente por tarea, sin fallback silencioso. Preferencia de economía: Claude Haiku 5.5 para tareas simples si es adecuado; escalar solo mediante una nueva decisión del Director.
- **Antes de cada ejecución:** verificar repositorio, commit base real, scopes de lectura/escritura, autenticación, identidad de ejecutable, límites y autorización explícita de Executor.
- **Después:** recoger diff, pruebas, SHA, rutas modificadas y evidencias; detener si se exceden los scopes. El éxito del proveedor **no** implica tarea completada.
- **GitHub:** sin push, merge, publicación ni ejecución remota automáticos sin autorización explícita pertinente.
- **No reutilizar** identidades de ejecución consumidas ni sobrescribir evidencias históricas.
- **Comando `ra`:** ChatGPT consulta la review queue y decide continuar, corregir o escalar. Una review RA por sí sola no autoriza acciones del Executor.

## 11. Primer entregable recomendado

**F0 — Base de JCVH:** repositorio React funcional, rutas de ejemplo, estructura neutra ampliable, `db.json` sintético y una prueba Jest que pase; sin IA ni integración externa todavía. Crear una tarea verificable con archivos permitidos y commit base real. Solo ejecutar después de revisar el plan y autorizar el trabajo.

## 12. Referencias originales facilitadas por el usuario

**Fuentes proporcionadas:** tres enlaces externos y una fotografía adjunta. Su inclusión documenta la procedencia del material de referencia, **no** implica que todo su contenido esté verificado ni que exista autorización para reproducirlo públicamente.

1. **Publicación de Facebook relacionada con JCVH:**  
   https://www.facebook.com/permalink.php?story_fbid=pfbid02dWmtC6MwN6q9eSkZkVySsx5GHrjtff3J1bNJUGKH8o5dW4cqqZogzkTy5UmEaDNVl&id=100064690088664&rdid=CpJ36dcvXaFpgBQn#
2. **Noticias ONU — artículo compartido para contexto temático:**  
   https://news.un.org/es/story/2022/12/1517632
3. **Facebook — Jóvenes contra la violencia Honduras:**  
   https://www.facebook.com/people/Jovenes-contra-la-violencia-Honduras/100064690088664/?ref=1
4. **Fotografía aportada por el usuario:** `references/decorando-mi-barrio-el-progreso.png` (incluida en el ZIP de entrega). En la imagen aparece el texto «Decorando mi barrio», la localidad «El Progreso, Yoro» y una referencia a una jornada de voluntarios de JCV y del Comité de Personas Refugiadas del Valle de Sula. Tratar esta descripción como **contenido visible en la imagen**, no como verificación institucional externa.

### Sedes o capítulos indicados por el usuario

- Tegucigalpa
- La Ceiba
- Comayagua
- El Progreso
- San Pedro Sula
- Siguatepeque (el usuario escribió también «Siaguatepeque»)

### Instrucciones para Orchestor sobre fuentes

- Usar las referencias para comprender las actividades, el contexto comunitario y los temas relevantes; **no** imponer su estética a la plataforma.
- Verificar antes de presentar como oficiales datos de contacto, ubicaciones exactas, autoridades, servicios, cifras o campañas.
- Mostrar fuentes vinculadas a los contenidos que efectivamente se utilicen. Si Facebook no permite acceso automatizado, registrar la limitación y avanzar con contenido demostrativo claramente identificado.
- **No publicar, copiar ni redistribuir la fotografía de personas o el material de Facebook** sin confirmar los permisos necesarios; en la primera demo utilizar contenido e imágenes con derechos claros o placeholders.
- No deducir relaciones jurídicas, patrocinios ni representación oficial de JCVH a partir de estos enlaces.

## 13. Pendientes de confirmar (no bloquear el diseño base)

- **Repositorio de destino seleccionado:** `Osarii/test_1` (https://github.com/Osarii/test_1), público y sin commits al inicio. Requiere una autorización separada para crear el commit inicial; no publicar fotografías identificables, credenciales ni datos sensibles.
- **Identidad visual:** **queda delegada a Orchestor** para la primera propuesta; no solicitar previamente al usuario colores, tipografías, layout ni imágenes de referencia adicionales. Uso público de logotipos/fotos institucionales sujeto a permisos.
- **Contenido institucional:** validar textos, contactos y datos de sedes antes de publicarlos como oficiales. Las tres URLs de referencia aportadas ya están incorporadas en la sección 12.
- **Proveedor externo y de IA:** seleccionar solo al activar integración, sujeto a acceso, seguridad y coste autorizado.
- **Despliegue:** entorno y dominio; no publicar el prototipo como servicio oficial sin aprobación.

**Definición de terminado:** MVP demostrable, basado en datos seguros/sintéticos, con pruebas y documentación; revisión final del Director y aprobación humana para cualquier publicación.
