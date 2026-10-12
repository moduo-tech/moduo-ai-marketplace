# Moduo AI Marketplace

Marketplace de skills y agentes agrupados por tecnología. Cada tecnología vive en `plugins/<tecnologia>/`; Claude Code y Codex comparten los `SKILL.md`, mientras que sus agentes y catálogos usan formatos propios.

## Estructura y compatibilidad

| Elemento | Fuente de verdad | Claude Code | Codex |
| --- | --- | --- | --- |
| Skills | `plugins/astro/skills/<nombre>/SKILL.md` | Carga las skills del plugin | Carga las mismas skills del plugin |
| Agentes | Revisores especializados por tecnología | `plugins/astro/agents/*.md` | Plantillas en `plugins/astro/codex/agents/*.toml`; hay que copiarlas al proyecto en `.codex/agents/` |
| Reglas del proyecto | `plugins/astro/rules/astro-project.md` | Integrarlas en `CLAUDE.md` o importar el `AGENTS.md` del proyecto | Integrarlas en el `AGENTS.md` del proyecto |
| Identidad y versión | `plugins/astro/plugin.json` | Mantener sincronizado `plugins/astro/.claude-plugin/plugin.json` | Usa el manifiesto portable |
| Catálogo | Una entrada por plugin | `.claude-plugin/marketplace.json` | `.agents/plugins/marketplace.json` |

Las [reglas de mantenimiento](./AGENTS.md), compartidas con Claude mediante [CLAUDE.md](./CLAUDE.md), indican cómo modificar una skill sin romper su uso en los dos clientes. Los campos compartidos del frontmatter son `name` y `description`; las opciones exclusivas de Claude deben quedarse fuera de las skills comunes. Claude y OpenAI documentan este formato de `SKILL.md`. [Claude Code](https://code.claude.com/docs/en/skills), [OpenAI Docs](https://developers.openai.com/plugins/build/skills).

### Plugin Astro

[`plugins/astro`](./plugins/astro/) incluye diseño y tokens, UI, tests, QA funcional y responsive, accesibilidad, rendimiento, contenido localizado, CMS y SEO. Sus agentes de revisión usan el [formato de informe común](./plugins/astro/references/qa-report.md); `astro-qa-coordinator` consolida los resultados y los huecos de cobertura. Consulta las [fuentes y atribuciones](./plugins/astro/SOURCES.md).

Las reglas de Astro son una plantilla para el proyecto de destino. Instalar el plugin no inserta automáticamente reglas en `AGENTS.md` o `CLAUDE.md`, ni copia las plantillas de agentes de Codex al proyecto.

## Modificar una skill

1. Edita el `SKILL.md` canónico dentro de `plugins/<tecnologia>/skills/`. Conserva un `name` igual al nombre de la carpeta y una `description` que delimite cuándo se usa. Coloca referencias, scripts o assets dentro del mismo plugin.
2. Mantén las instrucciones independientes del cliente. Si hace falta una función exclusiva de Claude o Codex, documenta la variante en su archivo específico y ofrece una alternativa clara en la skill compartida.
3. Si cambia el comportamiento de un revisor, actualiza el agente Claude, la plantilla Codex y, cuando corresponda, el [contrato de informes](./plugins/astro/references/qa-report.md) y el coordinador.
4. Valida JSON, TOML, frontmatter y enlaces relativos. Con Claude Code disponible, ejecuta `claude plugin validate ./plugins/astro` y `claude plugin validate .`; después comprueba la instalación o carga en ambos clientes. La validación de Claude comprueba el manifiesto y catálogo, pero la instalación detecta problemas adicionales. [Claude Code](https://code.claude.com/docs/en/plugin-marketplaces).

## Versionado y publicación

**Unidad de versión:** el plugin de cada tecnología. Las skills de Astro evolucionan juntas bajo `moduo-astro`; no añadimos un número de versión a cada `SKILL.md`. Los catálogos no tienen una versión semántica propia: un tag Git identifica una instantánea del marketplace completo. La versión actual `0.1.0` es un borrador y este repositorio aún no tiene etiquetas de release.

| Cambio | Política antes de `1.0.0` | Después de `1.0.0` |
| --- | --- | --- |
| Corrección editorial o de instrucciones sin cambio de flujo | Patch | Patch |
| Skill, agente o flujo nuevo; ampliación compatible | Minor | Minor |
| Cambio incompatible de nombre, ruta o contrato | Minor con migración documentada | Major |

En cada release, actualizamos **la misma versión** en `plugins/<tecnologia>/plugin.json` y `plugins/<tecnologia>/.claude-plugin/plugin.json`, anotamos los cambios visibles y etiquetamos el commit como `<plugin>-v<version>`; por ejemplo, `moduo-astro-v0.2.0`. No repetimos `version` en las entradas de los dos catálogos. Claude Code prioriza la versión de su manifiesto y, para instalaciones remotas, no entrega una edición con el mismo número de versión; los plugins locales cargados en el sitio siguen otra mecánica. [Claude Code: versiones y actualizaciones](https://code.claude.com/docs/en/plugins/host-marketplace), [OpenAI Docs: paquete y versión](https://developers.openai.com/plugins/build/plugins).

Las etiquetas Git permiten fijar **todo el marketplace** a un estado conocido. Claude acepta `moduo-tech/moduo-ai-marketplace#<ref>`; Codex permite `codex plugin marketplace add moduo-tech/moduo-ai-marketplace --ref <ref>`. Codex también permite actualizar una fuente Git con `codex plugin marketplace upgrade`. Con las rutas relativas actuales, el ref fija el catálogo completo; para versionar cada plugin de forma independiente haría falta una fuente Git por plugin o catálogos separados. [Claude Code: refs](https://code.claude.com/docs/en/plugins/host-marketplace), [OpenAI Docs: marketplaces](https://developers.openai.com/plugins/build/plugins).

### Pasos para publicar una release

1. **Revisa el alcance.** Comprueba qué skills, agentes y reglas cambian. Actualiza ambas variantes de cada agente afectado y los catálogos si añades un plugin nuevo. Anota los cambios visibles y cualquier migración que necesiten los usuarios.
2. **Elige la versión del plugin.** Aplica la tabla anterior y escribe exactamente el mismo valor en `plugins/astro/plugin.json` y `plugins/astro/.claude-plugin/plugin.json`. En la primera release puedes conservar `0.1.0`, porque todavía es un borrador sin tag publicado. No añadas una versión a cada skill ni a las entradas de `marketplace.json`.
3. **Valida antes de publicar.** Comprueba JSON, TOML, frontmatter YAML, enlaces relativos y nombres coincidentes; ejecuta `git diff --check`. Con Claude Code instalado, ejecuta `claude plugin validate ./plugins/astro` y `claude plugin validate .`. Comprueba que Codex descubre el plugin desde el catálogo local y que una skill carga en cada cliente; para probar un agente Codex, copia su plantilla a `.codex/agents/` de un proyecto de prueba. Si no tienes uno de los clientes, registra esa comprobación como pendiente en las notas de release.
4. **Crea el commit de release.** Incluye los manifiestos y las notas de cambio en un commit revisado. Integra y sube ese commit a la rama de publicación. Después crea un tag anotado e inmutable sobre el mismo commit y súbelo. Ejemplo para la primera release, una vez subida la rama:

   ```text
   git tag -a moduo-astro-v0.1.0 -m "moduo-astro 0.1.0"
   git push origin moduo-astro-v0.1.0
   ```

5. **Publica y comprueba.** Crea una GitHub Release desde el tag con cambios, migraciones y comprobaciones pendientes. Actualiza el marketplace en Claude Code o ejecuta `codex plugin marketplace upgrade` para refrescar la fuente Git de Codex; instala o actualiza `moduo-astro` y verifica una skill en una sesión nueva. No reutilices el número de versión ni muevas un tag ya publicado. [Claude Code: actualización](https://code.claude.com/docs/en/plugins/host-marketplace), [OpenAI Docs: fuentes Git](https://developers.openai.com/plugins/build/plugins).

## Instalación de prueba

Tras publicar el repositorio, Claude Code puede registrar el catálogo con `claude plugin marketplace add moduo-tech/moduo-ai-marketplace` e instalar `moduo-astro@moduo-marketplace`. Codex puede registrarlo con `codex plugin marketplace add moduo-tech/moduo-ai-marketplace` e instalar `moduo-astro` desde su navegador de plugins. Para pruebas locales, ambos clientes aceptan una ruta al repositorio; revisa los comandos disponibles en la versión instalada. [Claude Code](https://code.claude.com/docs/en/plugin-marketplaces), [OpenAI Docs](https://developers.openai.com/plugins/build/plugins).
