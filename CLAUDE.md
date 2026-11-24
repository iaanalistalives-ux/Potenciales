# CLAUDE.md — Potenciales de Planta (Potenciales)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
Potencial = porcentaje de habilidad que el sistema registra para cada persona, por semana, en tres áreas: COSTURA, CORTE y ACABADOS. No hay meta fijada.
- `index.html` — **tablero** de solo lectura para Gerencia: filtros de área (Planta, Costura, Corte, Acabados), vista Semana o Acumulado, semana y línea/proceso. Pestañas Resumen, Personas, Máquinas y operaciones, Dónde atacar primero y Datos del sistema; ficha de tendencia por persona. Regla 500 en la tabla de datos. CSV del período, Excel LIVES e Imprimir (A4 horizontal).
  - Botón **«Subir archivos»** en la barra de filtros (desde el 24/11/2026): abre `carga.html` en una ventana dentro del tablero (casi pantalla completa; completa en celular), con «✕ Cerrar». Si quedó un archivo sin guardar pregunta «¿Salir sin guardar?»; si se guardó algo, el tablero vuelve a leer los datos al cerrar. El botón pregunta a la base (`potenciales_puede_subir`): puede, activo; no puede o sin sesión, gris («Solo para quien sube archivos»).
- `carga.html` — **carga semanal** (se abre desde el botón del tablero y conserva su link directo): un cuadro por área; cada una sube su archivo tal como sale del sistema y presiona "Guardar <Área>". Volver a subir actualiza, no duplica.
  - Costura: `Rpt_Potencial_Maquinista` (.xls), formato largo con año y semana propios.
  - Corte y Acabados: cuadros "POTENCIALES DE CORTE / ACABADOS" (.xlsx), formato ancho con columnas `SEM nn`; el año se elige en pantalla. Un valor ≤ 3 se toma como fracción y se multiplica por 100.
  - Lee todas las hojas visibles e ignora filas ocultas; avisa si el archivo es de otra área.

## Base de datos (Supabase lives-datos)
- Leer: tabla `potenciales` directo (de 1000 en 1000). `potenciales_puede_ver` solo se consulta si no llegan filas, para distinguir "sin permiso" de "sin datos".
- Cargar: `potenciales_cargar` (lotes de 1000 filas; tablas `potenciales` y `potenciales_cargas`).
- No usa Storage.

## Quién la usa
Según las entradas por el portal de los últimos 30 días al 21/10/2026: 4 personas (supervisor de planta, supervisora de Mejoras, analista de Ingeniería en Corte y Datos e IA).

## Quién puede cargar
- Lo decide la base: `potenciales_puede_subir` (aviso al abrir) y `potenciales_cargar` vuelve a revisar al guardar, con la tabla `permisos`. Sin permiso la página se ve, pero no guarda.
- El permiso se enciende y apaga por persona en el panel «Quién registra» (`lives-portal/admin.html`), opción **«Potenciales · cargar»** (`registra_herramientas.clave = 'pot'`; guarda en `permisos` el link de `carga.html`). El mismo interruptor manda sobre el botón «Subir archivos» del tablero y sobre el guardado.
- A Sistemas: `potenciales_puede_subir` es la pieza que reemplazan con su seguridad.
