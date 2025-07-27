# Ejemplos útiles en Git

## Ignorar archivos con .gitignore

Crea un archivo `.gitignore` y agrega los archivos o carpetas que no quieres versionar:

```gitignore
node_modules/
.env
.DS_Store
```

## Revertir el último commit sin perder los cambios

```bash
git reset --soft HEAD~1
```

## Eliminar archivos del seguimiento pero mantenerlos localmente

```bash
git rm --cached archivo.txt
```

## Cambiar el mensaje del último commit

```bash
git commit --amend -m "Nuevo mensaje"
```
