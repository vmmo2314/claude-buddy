# claude-buddy

Fork de [claude-familiar](https://github.com/lucenity0/claude-familiar) (MIT, de Nafees S): una mascota de pixel art que vive encima del prompt de Claude Code. La documentación original está en [README.upstream.md](README.upstream.md).

## Qué agrega este fork

- **Varias mascotas guardadas.** Comandos nuevos:
  ```
  /familiar save <nombre>     guarda el look actual con ese nombre
  /familiar wear <nombre>     se pone un look guardado
  /familiar saved             lista los looks guardados
  /familiar forget <nombre>   borra uno
  ```
- **El selector `/familiar species` muestra los looks guardados** como tarjetas junto a los que trae el plugin. Clic o número para ponérselo.
- **Skill `mascota-familiar`**: pídele a Claude "hazme una mascota de ..." y diseña el sprite 16x12, lo valida y lo agrega a `mascotas-familiar.txt` en tu Escritorio con el `/familiar import ...` listo para pegar.
- **Mascotas de ejemplo** en [mascotas/mascotas-familiar.txt](mascotas/mascotas-familiar.txt): Clawd con chullo, Clawd en llama y cuy con poncho.

Los cambios de código están en `hooks/register.tsx` y la prueba en `hooks/familiar.test.ts`.

## Instalación (instrucciones para un agente)

Ejecuta estos pasos en orden.

1. Activar los function hooks (son early-access). Agregar a `~/.claude/settings.json`, conservando lo que ya tenga:
   ```json
   { "env": { "CLAUDE_CODE_ENABLE_FUNCTION_HOOKS": "1" } }
   ```
2. Si ya está instalado el plugin original, desactivarlo para que no salgan dos mascotas:
   ```sh
   claude plugin uninstall familiar@claude-familiar
   ```
   (Si el usuario quiere conservar su mascota y xp del original, en vez de desinstalarlo se puede copiar `hooks/register.tsx` de este repo sobre `~/.claude/plugins/cache/claude-familiar/familiar/<versión>/hooks/register.tsx`. Ese parche se pierde si el plugin original se actualiza.)
3. Instalar este fork:
   ```sh
   claude plugin marketplace add vmmo2314/claude-buddy
   claude plugin install familiar@claude-buddy
   ```
4. Pedirle al usuario que abra una sesión nueva de Claude Code. La mascota aparece encima del prompt.
5. Opcional: cargar las mascotas de ejemplo. Por cada bloque de `mascotas/mascotas-familiar.txt`, el usuario pega su línea `/familiar import ...` y luego `/familiar save <nombre>`.

La skill `mascota-familiar` viene dentro del plugin, no hay que copiarla aparte.

Probado en Claude Code 2.1.292 en Windows. Los function hooks pueden cambiar entre versiones.

## Desarrollo

```sh
claude plugin validate .
claude plugin test .
```
