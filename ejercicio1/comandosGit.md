# **Hoja Resumen: Git y GitHub Comandos**

---

## **Comandos básicos**

- **Inicializar repositorio local**:
  ```bash
  git init
  ```
- **Clonar un repositorio**:
  ```bash
  git clone <url>
  ```
- **Ver estado de cambios**:
  ```bash
  git status
  ```
- **Añadir archivos al área de preparación (stage)**:
  ```bash
  git add <archivo> 
  git add .  # Todos los cambios
  ```
- **Hacer un commit**:
  ```bash
  git commit -m "mensaje"
  ```
- **Cambiar mensaje de un commit reciente**:
  ```bash
  git commit --amend -m "nuevo mensaje"
  ```
- **Sincronizar cambios con el remoto**:
  ```bash
  git pull
  git push
  ```
- **Hacer commit de lineas especificas**
```bash
git add -p nombreArchivo
```

- **Diferrencia entre .add y no stage**
```bash
git diff --cached
```

---

## **Gestión de ramas**

- **Crear y cambiar a una nueva rama**:
  ```bash
  git checkout -b <nombre_rama>
  ```
- **Listar ramas existentes**:
  ```bash
  git branch
  ```
- **Eliminar una rama**:
  ```bash
  git branch -d <nombre_rama>
  ```
- **Fusionar ramas**:
  ```bash
  git merge <rama_origen>
  ```
- **Ver ramas remotas**:
  ```bash
  git branch -r
  ```

---

## **Resolución de conflictos**

1. Identificar conflicto:
   ```bash
   git status
   ```
2. Editar archivo con conflicto y decidir qué cambios mantener:
   ```text
   <<<<<<< HEAD
   # Cambios locales
   =======
   # Cambios remotos
   >>>>>>> rama_remota
   ```
3. Marcar conflicto como resuelto:
   ```bash
   git add <archivo>
   git commit -m "fix: Resuelve conflictos"
   ```

---

## **Revertir y deshacer cambios**

- **Revertir un commit específico**:
  ```bash
  git revert <hash>
  ```
  Esto genera un nuevo commit que revierte los cambios del commit especificado. Útil para mantener historial limpio.

- **Reset** (volver a un estado previo):
  - **Soft** (mantiene los cambios en stage):
    ```bash
    git reset --soft <hash>
    ```
  - **Mixed** (mantiene los cambios en el área de trabajo, pero los quita del stage):
    ```bash
    git reset --mixed <hash>
    ```
  - **Hard** (elimina completamente los cambios):
    ```bash
    git reset --hard <hash>
    ```
  ⚠️ **Precaución:** `--hard` elimina todos los cambios, incluso los no confirmados.

- **Comparar estados antes y después del reset**:
  ```bash
  git diff <hash>
  ```

---

## **Comandos avanzados**

- **Fetch** (traer cambios remotos sin fusionarlos):
  ```bash
  git fetch
  ```
  Actualiza las referencias remotas locales con los cambios del repositorio remoto. No afecta la rama actual, permitiendo inspeccionar antes de fusionar.

- **Cherry-pick** (aplicar un commit específico de otra rama):
  ```bash
  git cherry-pick <hash>
  ```
- **Rebase interactivo (combinar commits o reorganizar el historial)**:
  ```bash
  git rebase -i HEAD~<número_commits>
  ```
- **Stash (guardar cambios no confirmados temporalmente)**:
  ```bash
  git stash
  git stash apply  # Restaurar sin borrar de la pila
  git stash pop    # Restaurar y eliminar de la pila
  git stash list   # Mostrar los cambios guardados
  ```
- **Ver diferencias entre commits**:
  ```bash
  git diff <commit1> <commit2>
  ```
- **Mostrar el historial del repositorio**:
  ```bash
  git log --oneline --graph --all
  ```

---

## **Ver y corregir errores**

- **Ver diferencias no confirmadas**:
  ```bash
  git diff
  ```
- **Diferencias en el área de preparación**:
  ```bash
  git diff --cached
  ```
- **Deshacer cambios no confirmados en un archivo**:
  ```bash
  git checkout -- <archivo>
  ```

---

## **Configuración útil**

- **Configurar nombre y correo para el repositorio**:
  ```bash
  git config user.name "Tu Nombre"
  git config user.email "tu_email@ejemplo.com"
  ```
- **Cambiar la URL de un remoto**:
  ```bash
  git remote set-url origin <nueva_url>
  ```

---

## **Ejercicios prácticos destacados**

1. **Crear Pull Request**:
   1. Sube los cambios de tu rama:
      ```bash
      git push -u origin <nombre_rama>
      ```
   2. En GitHub: Ve a **Pull requests** → **New Pull Request**.
   3. Selecciona la rama base y la rama con los cambios.

2. **Resolución de conflictos**:
   - Cambia entre ramas y provoca un conflicto modificando la misma línea en ambas ramas. Usa `git merge` para enfrentarte al conflicto.

3. **Cherry-pick**:
   - Identifica el hash del commit:
     ```bash
     git log --oneline
     ```
   - Aplica el commit en tu rama actual:
     ```bash
     git cherry-pick <hash>
     ```

4. **Uso de Stash**:
   - Guarda cambios no confirmados:
     ```bash
     git stash
     ```
   - Recupera los cambios:
     ```bash
     git stash apply
     ```

5. **Revertir cambios accidentalmente eliminados**:
   - Usa `git revert` para reponer cambios perdidos sin alterar el historial general.