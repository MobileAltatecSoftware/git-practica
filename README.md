
# Flujo de trabajo Git con ramas main, develop e integration

Este repositorio documenta un flujo de trabajo Git personalizado que organiza el desarrollo en tres ramas principales: `main`, `develop` e `integration`. Está diseñado para mantener un historial limpio, controlado y facilitar pruebas antes de pasar a producción.

## 🧱 Ramas principales

- `main`: Rama de producción. Contiene versiones estables listas para liberación.
- `develop`: Rama base para las siguientes versiones. Acumula cambios ya probados.
- `integration`: Rama de staging donde se integran y prueban funcionalidades antes de promoverlas a `develop`.

## 🔄 Flujo de trabajo paso a paso

### 1. Crear una rama personal desde `integration`

```bash
git checkout integration
git checkout -b feature/mi-nueva-funcionalidad
```

---

### 2. Desarrollar la funcionalidad

Realiza los cambios necesarios y haz los commits correspondientes. Se recomienda mantener commits pequeños y significativos.

```bash
git add .
git commit -m "Agrega nueva funcionalidad X"
```

Prueba localmente tu código antes de integrarlo.

---

### 3. Integrar cambios usando `cherry-pick` desde `integration`

Una vez que la funcionalidad esté lista y probada, ve a la rama `integration` y selecciona solo los commits deseados usando `cherry-pick`:

```bash
git checkout integration
git cherry-pick <hash1> <hash2> ...
```

Puedes obtener los hashes de los commits con:

```bash
git log --oneline
```

---

### 4. (Opcional) Realizar squash de commits

Si la funcionalidad tiene múltiples commits, puedes combinarlos en uno solo para mantener el historial limpio:

```bash
git rebase -i HEAD~n
```

En el editor interactivo, deja el primer commit como `pick` y cambia los siguientes a `squash` o `s`.

---

### 5. Eliminar la rama personal

Una vez que la funcionalidad esté integrada correctamente en `integration`:

```bash
git branch -d feature/mi-nueva-funcionalidad
```

---

### 6. Probar en `integration`

Asegúrate de que la funcionalidad integrada funcione correctamente y no haya roto nada. Si es posible, realiza pruebas automáticas o revisión de código.

---

### 7. Rebase hacia `develop`

Cuando el conjunto de funcionalidades esté listo en `integration`, se hace un rebase hacia `develop` para acumular las versiones en desarrollo:

```bash
git checkout develop
git rebase integration
```

---

### 8. Merge a `main` y etiquetado

Cuando todo esté probado y aprobado en `develop`, se puede promover a producción con un merge a `main` y asignando un tag de versión:

```bash
git checkout main
git merge develop
git tag v1.0.0
git push --tags
```

---

## ✅ Ventajas de este flujo

- 🧼 **Historial limpio**: Gracias a `cherry-pick` y `squash`.
- 🧪 **Separación clara** entre trabajo en desarrollo, en pruebas y estable.
- 🎯 **Control preciso** de qué cambios entran a cada rama.
- 🧹 **Ramas personales eliminadas** tras su integración, lo que mantiene el repositorio ordenado.
- 🧷 **Estabilidad en `develop`** al evitar merges prematuros.

---

## ⚠️ Consideraciones y desventajas

- 🔁 El uso frecuente de `cherry-pick` puede generar conflictos si no se aplica con cuidado.
- 🧭 Se pierde trazabilidad directa de commits tras un `squash`.
- 🧠 Es más complejo de mantener que flujos como Git Flow o GitHub Flow.
- 🧍 Requiere disciplina y entendimiento por parte de todo el equipo.

---

## 🧠 ¿Este flujo es adecuado para tu equipo?

Sí, siempre que:

- Todo el equipo comprenda y respete el flujo.
- Se mantenga documentado.
- Aporte valor real (por ejemplo, al separar fases de desarrollo y pruebas).

---

## 📄 Licencia

Este repositorio es de uso educativo y demostrativo. Siéntete libre de adaptarlo a tu equipo o proyecto.
