# Codex workflows

Workflows reutilizables de GitHub Actions para convertir issues etiquetados en PRs y responder a revisiones que solicitan cambios.

## Contenido

- `solve-labeled-issue.yml`: implementa un issue abierto cuando recibe un label. Crea o actualiza un draft PR y recupera una rama ya creada si el PR falló.
- `address-requested-review.yml`: responde solo a una revisión formal de GitHub con estado **Request changes**. Lee los comentarios inline, modifica la rama del PR y deja una respuesta.
- `examples/`: dispatchers mínimos que deben copiarse en cada repositorio consumidor.

## Publicar este repositorio

1. Crea un repositorio privado llamado `codex-workflows` en tu cuenta GitHub.
2. Sube el contenido de esta carpeta.
3. En el repositorio central, ve a **Settings → Actions → General** y permite que los repositorios privados de tu cuenta puedan acceder a sus workflows reutilizables.
4. Crea y publica un tag de versión, inicialmente `v1`.

El repositorio central no necesita `OPENAI_API_KEY`.

## Configurar un repositorio consumidor

1. En el repositorio consumidor, añade el secret Actions `OPENAI_API_KEY`.
2. En **Settings → Actions → General → Workflow permissions**, permite que GitHub Actions cree pull requests.
3. Copia uno o los dos archivos de `examples/` a `.github/workflows/` en el repositorio consumidor.
4. Reemplaza `marcomilon/codex-workflows@v1` si usas otro propietario, repositorio o versión.

Los dispatchers pasan únicamente la API key que el workflow necesita. El `GITHUB_TOKEN` conserva los permisos del repositorio consumidor; esos permisos se declaran en cada dispatcher.

## Uso

- Añade cualquier label a un issue abierto para lanzar `solve-labeled-issue`.
- En un PR originado en el mismo repositorio, envía una revisión de tipo **Request changes** para lanzar `address-requested-review`.

Los workflows nunca hacen merge. El workflow de review ignora aprobaciones y comentarios normales para evitar consumo innecesario de la API.
