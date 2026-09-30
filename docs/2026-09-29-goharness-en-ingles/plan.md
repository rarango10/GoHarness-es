# Plan — GoHarness en inglés, en un repo propio

> **Estado: aprobado (2026-09-29).**
>
> Este es el último plan de este repo. La fase 0 se ejecuta acá; de la fase 1 en adelante el
> trabajo vive en el repo nuevo, que arranca con una copia de este plan como su documento
> fundacional. Esta copia no se actualiza después: queda como el **porqué** de la mudanza.

## Contexto

GoHarness se probó con éxito en español, sobre la calculadora de este repo. Ahora hay compañeros de
trabajo de habla inglesa que lo van a **usar, leer y proponerle cambios**. Eso último es lo que
decide: nadie puede opinar sobre una regla que no puede leer, así que las instrucciones del harness
—no solo sus bordes— tienen que estar en inglés.

El plan del [2026-09-12](../2026-09-12-modos-de-trabajo/plan.md#cuándo-sí-convendría-partir-el-repo)
dejó fechada esta decisión y sus señales. La segunda se cumplió: *«que aparezca gente de afuera
contribuyendo al harness y el ruido del ejemplo moleste»*. Ese plan también anticipó el destino de
`HARNESS.md`: convertirse en el README del repo del mantenedor.

**El repo lo usa solo su autor**; una o dos personas lo exploraron. No hay usuarios a quienes
mantenerles compatibilidad: alcanza con avisar.

**Objetivo:** un repo `GoHarness` nuevo, en inglés, dedicado solo al plugin, que también funcione
en español; y este repo, renombrado `GoHarness-es`, congelado en la 0.5.2 como caso de estudio.

## Las opciones que se evaluaron

| Opción | Por qué no |
|---|---|
| **Fork en inglés, con los dos vivos** | Cada mejora hecha dos veces, a mano, sin nada que detecte cuándo se separan. Al ritmo de este harness (7 commits al plugin en un solo día) el inglés viviría atrasado. |
| **Todo el harness bilingüe, en este repo** | El mismo doble mantenimiento, en un solo lugar. Y los plugins de Claude Code no tienen forma de cambiar de idioma. |
| **Instrucciones en español, bordes en inglés** | Servía si los compañeros solo lo *usaban*. Como también van a proponer cambios, no alcanza. |
| **Pasar este repo a inglés, en el lugar** | Viable, pero arrastra la calculadora, su contrato en español y la compatibilidad con proyectos 0.5.x. |
| **Repo nuevo en inglés + este congelado** ✔ | Una sola rama viva, así que no hay doble mantenimiento. Y el repo nuevo nace sin la confusión de «dos cosas a la vez» que hoy resuelve `EMPEZAR-ACA.md`. |

## Decisiones tomadas

1. **El nombre `GoHarness` pasa al repo nuevo.** Este se renombra `GoHarness-es`, y su plugin pasa
   a llamarse `goharness-es`. Los dos plugins no pueden llamarse igual: «dos plugins con el mismo
   nombre no conviven, y el que pierde se apaga en silencio» ([`HARNESS.md`](../../HARNESS.md)).
2. **`GoHarness-es` queda congelado de verdad.** Su último commit es el de la fase 0. Si recibe
   arreglos, se convierte en el fork de doble mantenimiento que se descartó.
3. **El harness nuevo habla inglés por dentro y el idioma del proyecto por fuera.** El autor
   trabaja en español y va a usar el harness nuevo, así que el español es un idioma soportado, no
   una herencia.
4. **La calculadora no se muda.** Queda en `GoHarness-es` como caso de estudio público, y el README
   nuevo la enlaza.
5. **Las lecciones se destilan, no se traducen.** Las 3.283 líneas de [`lecciones.md`](../../lecciones.md)
   quedan acá. Al repo nuevo viaja un resumen de principios, con un enlace a cada entrada original,
   más las que siguen abiertas.
6. **Mientras dura la mudanza no entran mejoras al harness.** Lo que aparezca se anota como lección
   nueva y se aplica después de la fase 6. Mezclar traducción con cambios de comportamiento hace
   imposible saber qué causó qué: es la regla de «no editar el harness con una corrida en vuelo»,
   aplicada a la mudanza.

## Fase 0 — Cerrar `GoHarness-es` (en este repo)

El orden importa por una razón concreta. **GitHub redirige el nombre viejo de un repo renombrado
hasta que alguien crea un repo nuevo con ese nombre; en ese momento la redirección se corta.** Todo
lo que dependa de la redirección tiene que estar hecho y verificado antes del paso 6.

1. **Último commit** (versión **0.5.2**):
   - `plugin.json` y `.claude-plugin/marketplace.json`: `name` → `goharness-es`.
   - Instrucciones de instalación → `rarango10/GoHarness-es` y `goharness-es@goharness-es`, en
     `README.md`, `EMPEZAR-ACA.md` y `HARNESS.md`. Los planes viejos no se tocan: son registro.
   - Aviso al principio del `README.md`: *«Esta versión quedó congelada en la 0.5.2. La evolución
     sigue en [rarango10/GoHarness](https://github.com/rarango10/GoHarness), que también funciona
     en español.»*
   - Nota al principio del índice de `lecciones.md`: el repo quedó congelado; las lecciones nuevas
     se escriben en el repo nuevo.
   - Las cinco verificaciones de `HARNESS.md` en verde. La guarda de paridad no debería moverse:
     no cambia ninguna regla.
2. **Renombrar el repo en GitHub:** Settings → Repository name → `GoHarness-es`.
3. **Actualizar el clon local:** `git remote set-url origin https://github.com/rarango10/GoHarness-es.git`.
   **Antes de crear el repo nuevo**: si no, un `git push` desde acá mandaría la historia de la
   calculadora al repo equivocado.
4. **Push de `main`, y después el tag** (`claude plugin tag plugin/goharness --push`), en ese orden,
   como dice `HARNESS.md`.
5. **Instalación real en una carpeta descartable** de `goharness-es@goharness-es` desde el nombre
   nuevo. Mirar que `installPath` termine en `0.5.2`.
6. **Limpiar la máquina:** desinstalar `goharness@goharness`, quitar el marketplace viejo y borrar la
   copia de desarrollo `~/.claude/skills/goharness/`. Esa ruta la va a usar el plugin nuevo.
7. **Renombrar la carpeta local** `dev/GoHarness` → `dev/GoHarness-es`. Ojo: la memoria de las
   sesiones de Claude está atada a la ruta. Si el repo nuevo se clona en `dev/GoHarness`, hereda
   esas notas, que es lo que se quiere, porque son preferencias del autor y no del repo.

**Compuerta:** el paso 5 en verde. Recién ahí se crea el repo nuevo.

## Fase 1 — Nace el repo nuevo

- **Crear `rarango10/GoHarness`** con la historia de `plugin/` y `.claude-plugin/`, sin la
  calculadora, para que `git blame` siga explicando cada línea del plugin. Se hace con
  `git filter-repo --path plugin/ --path .claude-plugin/` (hay que instalarlo) o con
  `git subtree split`; se decide al ejecutarla. Si ninguna anda limpio, se arranca sin historia y
  se enlaza al repo viejo.
- **Misma estructura** `plugin/goharness/`, así los comandos de verificación y el sync no cambian de
  ruta.
- **Copia de este plan** como `docs/<fecha>-founding-plan/plan.md`, traducida.
- **Banco de pruebas** (ver la sección siguiente), antes de tocar una sola instrucción.
- **Sin archivo de ruteo.** `EMPEZAR-ACA.md` existía porque el repo era dos cosas; el nuevo es una.

## El banco de pruebas que reemplaza a la calculadora

La calculadora cumplía dos funciones: **vidriera** y **banco de pruebas**. La vidriera queda
resuelta con el enlace al caso de estudio. El banco de pruebas necesita un reemplazo explícito,
porque ahí nacieron casi todas las lecciones.

- **Un proyecto descartable por prueba, que arranca en el paso 0.** Es mejor que la calculadora: su
  `CLAUDE.md` tenía semanas, así que `harness-init` casi no volvía a correr desde cero.
- **Una receta escrita** en el `MAINTAINING.md` (ex `HARNESS.md`): cómo crear la carpeta, instalar
  el plugin local, qué feature chica pedir y qué mirar en cada paso.
- **Los evals existentes** de `brainstorming` y `specify` como red rápida. Se corren **antes** de
  traducir, para tener la línea base contra la que se compara después.

## Fase 2 — Bases que no dependen del idioma (el texto sigue en español)

Primero la estructura, después la prosa. Al terminar esta fase, el harness se comporta igual que
la 0.5.2 en un proyecto en español. Eso se prueba en el banco.

- **Etiquetas en las reglas:** cada regla de la plantilla y del router lleva `<!-- regla: <id> -->`,
  como ya hacen los casilleros con `<!-- ranura: … -->`. `check-rules-parity.cjs` compara etiquetas,
  no títulos en negrita. Sin eso, la guarda no puede comparar una plantilla en inglés con una en
  español.
- **Glosario de palabras clave**, en `formato-de-tareas`. Una versión canónica en inglés (`pending`,
  `in progress`, `done`, `meets`, `partially-meets`, `does-not-meet`, `unverifiable`, `Covers`,
  `Log`, `approved`…) con sus alias en español (`pendiente`, `en curso`, `hecho`, `cumple`, `Cubre`,
  `Registro`, `aprobado`…).
- **Esquemas en inglés canónico:** los `enum` de `tasks-fanout.js` pasan a los valores canónicos, y
  `spec-scout` normaliza lo que lee de un `tasks.md` en cualquiera de los dos idiomas.
- **`check_specs.py`** acepta los nombres de sección del design en los dos idiomas.
- **La guarda de paridad pierde un lado:** sin el `CLAUDE.md` de un ejemplo, compara router ↔
  plantilla, y plantilla en inglés ↔ plantilla en español.

## Fase 3 — Traducir el núcleo

- Skills, agentes, prompts del workflow y mensajes de los scripts (`e2e-doctor.cjs` le habla a quien
  usa el harness), **de a uno por vez**, con el banco de pruebas entre uno y otro.
- **Regla nueva:** conversar y escribir documentos en el idioma del proyecto, y traducir al decirlos
  los mensajes armados que traen los skills.
- **`description` de cada skill** en inglés, con ejemplos en los dos idiomas: «let's implement T3 /
  implementemos T3».
- **La traducción no cambia comportamiento.** Frases como «`hecho` significa verificado» o «no
  confundas "no pude verificar" con "no cumple"» salieron de lecciones concretas. Cuando la
  traducción obvia pierde el matiz, la entrada original de `lecciones.md` dice qué tiene que
  sobrevivir.
- **Evals después de traducir**, comparados contra la línea base de la fase 1.

## Fase 4 — Plantillas en los dos idiomas

- `assets/en/` y `assets/es/` para las seis plantillas: `CLAUDE`, `requirements`, `design`,
  `tasks`, el plan e2e y `pendientes` (~600 líneas).
- **Casillero `<!-- ranura: idioma -->`** en la plantilla del contrato. `harness-init` lo pregunta
  en el paso 0 y siembra las plantillas del idioma elegido.
- La guarda de paridad verifica que las dos versiones tengan las mismas etiquetas y los mismos
  casilleros.

## Fase 5 — Documentos del repo

| Archivo | De dónde sale |
|---|---|
| `README.md` | Las partes del README actual que le hablan a quien usa o evalúa el harness. «Momentos donde el ciclo hizo su trabajo» se resume y enlaza al caso de estudio en `GoHarness-es`. |
| `MAINTAINING.md` | `HARNESS.md`, más la receta del banco de pruebas. |
| `LESSONS.md` | Los principios destilados de lo que ya está resuelto, en inglés, con un enlace a la entrada original. Las entradas que siguen en pie viajan completas: **L6**, **L8**, **L12**, **L29**, **L50** y **L53**. Las lecciones nuevas se escriben en inglés. |
| `CLAUDE.md` | Uno corto para quien mantiene: el repo es el plugin, dónde está la fuente y qué se verifica. Ya no es el contrato de un ejemplo. |

## Fase 6 — Compuerta para compartir

1. **Una feature completa en inglés y otra en español**, cada una en su carpeta descartable,
   con el plugin instalado desde el marketplace real. Pasa si:
   - cada una recorre los nueve pasos;
   - los documentos salen en el idioma del proyecto;
   - ninguna palabra clave se escapa en el otro idioma.
2. **Versión 0.6.0.** Sigue la numeración, porque la historia del plugin viaja con el repo. La 1.0
   queda para cuando el harness deje de cambiar tan seguido.
3. **Recién ahí**, compartirlo con los compañeros.

## Qué no se hace

- No se mudan la calculadora ni sus `docs/`.
- No se traducen las entradas viejas de `lecciones.md`.
- No se toca `GoHarness-es` después de la fase 0.
- No se aplican mejoras al harness durante la mudanza (decisión 6).

## Riesgos

| Riesgo | Qué lo contiene |
|---|---|
| Crear el repo nuevo antes de verificar el renombre | La compuerta de la fase 0 |
| Pushear la calculadora al repo nuevo | Paso 3 de la fase 0, antes de crear nada |
| La traducción cambia el comportamiento sin que se note | Evals antes y después, banco entre skill y skill, y las lecciones como referencia de qué tiene que sobrevivir |
| Una palabra clave distinta entre agentes | Glosario único y esquemas canónicos (fase 2) antes de traducir la prosa |
| Las plantillas en los dos idiomas se separan | Guarda de paridad por etiquetas (fases 2 y 4) |
| Perder el banco de pruebas | Se arma en la fase 1, antes de cualquier cambio |
