# Flujo de trabajo recomendado en Git

## 1. Crear una rama para cada nueva funcionalidad o bugfix

```bash
git checkout -b feature/nueva-funcionalidad
```

## 2. Realizar cambios y hacer commits descriptivos

```bash
git add .
git commit -m "Agrega login de usuario"
```

## 3. Sincronizar cambios con el remoto

```bash
git push origin feature/nueva-funcionalidad
```

## 4. Crear un Pull Request (PR) en la plataforma correspondiente (GitHub/GitLab)

## 5. Revisar y fusionar (merge) la rama tras aprobación

## Consejos

- Mantén las ramas actualizadas con la rama principal (`main` o `master`).
- Realiza commits pequeños y descriptivos.
- Elimina ramas que ya han sido fusionadas.
