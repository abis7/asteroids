---
description: Crea un git worktree en .worktrees/ a partir de un argumento.
agent: build
---

Deriva el nombre del worktree desde el argumento recibido ($ARGUMENTS):

1. Si el argumento no tiene espacios, úsalo literal como nombre.
2. Si tiene espacios: minúsculas, espacios -> guiones (-), elimina cualquier
   carácter inválido para un nombre de directorio (ej: "Nueva vista login" -> "nueva-vista-login").
3. Sin prefijos, sufijos, ni ramas adicionales.
4. Si el argumento es muy largo, simplificalo a un nombre significativo.

Ejecuta EXACTAMENTE este comando, sin cambiar de directorio y sin hacer nada más:

git worktree add .worktrees/<nombre-derivado>