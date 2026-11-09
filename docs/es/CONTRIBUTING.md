# Cómo Contribuir a Arch Linux Agent Skill

[English](../../CONTRIBUTING.md) | [Português](../pt-br/CONTRIBUTING.md) | [Español](CONTRIBUTING.md)

¡Gracias por tu interés en contribuir a **Arch Linux Multi-Environment Agent Skill**! 🎉

Este repositorio proporciona conocimiento estandarizado, abierto y con límites de seguridad claros para asistentes de código con IA (Gemini, Claude, Antigravity y Open Agent Skills) que administran sistemas de escritorio Arch Linux.

---

## 🌟 Áreas de Colaboración

1. **Añadir Nuevos Gestores de Ventanas y Entornos:**
   * Soporte para *River*, *bspwm*, *AwesomeWM*, *Cosmic Desktop*, *Qtile*, *XFCE*, etc.
2. **Actualizar Guías Existentes:**
   * Mantenerse al día con cambios de sintaxis en Hyprland, Waybar, KWin o Mutter.
3. **Mejorar Automatizaciones y Scripts:**
   * Servicios de usuario de systemd, reglas udev y listeners IPC de compositores.
4. **Ampliar Recetas de Temas:**
   * Plantillas para paletas populares (Catppuccin, Tokyo Night, Nord, Gruvbox) en terminales, barras y aplicaciones GTK/Qt.

---

## 📐 Estándares para Skills de Agentes de IA

Las skills de IA requieren documentación concisa, sin ambigüedades y determinista:

### 1. Reglas de Seguridad No Destructivas
* **Prioridad al espacio de usuario:** Indicar a los agentes que guarden configuraciones estrictamente en `~/.config/` o `~/.local/bin/`.
* **Copias de Seguridad Automáticas:** Incluir siempre pasos de respaldo (`cp archivo archivo.bak.$(date +%s)`) antes de modificar archivos.
* **Separación de Privilegios en AUR:** Enfatizar siempre que los gestores de AUR (`yay`, `paru`) **nunca deben ejecutarse con `sudo`**.

### 2. Pureza Upstream
* Evitar wrappers propietarios o específicos de distribuciones derivadas.
* Utilizar herramientas estándar de Arch Linux (`pacman`, `systemd --user`, formatos nativos `.conf`, `.jsonc`, `.kdl`).

### 3. Bloques de Código Claros
* Proporcionar comandos CLI verificados y fragmentos de configuración con resaltado de sintaxis (por ejemplo, ````ini````, ````bash````, ````jsonc````, ````css````, ````kdl````).

---

## 📝 Plantilla para Nuevos Gestores de Ventanas / DEs

Si vas a añadir un nuevo entorno (por ejemplo, `river.md` o `cosmic.md`), sigue esta estructura:

```markdown
# Guía de [Nombre del Entorno] (Arch Linux)

Esta guía cubre la configuración y administración de **[Nombre]** en Arch Linux.

---

## 1. Ubicación de Archivos de Configuración
* Configuración principal: `~/.config/[app]/...`
* Subconfiguraciones modulares (si corresponde)

## 2. Pantallas y Escalado
* Cómo consultar y configurar resoluciones, tasas de refresco y escalas.

## 3. Atajos Principais y Reglas de Ventana
* Sintaxis para aplicaciones, navegación de ventanas y reglas de flotación.

## 4. Inicio de Daemons e Integración de Barra de Estado
* Cómo iniciar barras (Waybar/Polybar), notificaciones y bloqueo de pantalla.

## 5. Validación y Recarga
* Comandos para recargar la configuración sin reiniciar la sesión.
```

---

## 🛠️ Flujo de Desarrollo y Envío

### 1. Fork & Rama (Branch)
```bash
# Clona tu fork
git clone https://github.com/TU_USUARIO/arch-linux-skill.git
cd arch-linux-skill

# Crea una rama para la funcionalidad
git checkout -b feat/add-river-wm
```

### 2. Convención de Commits
Seguimos el estándar [Conventional Commits](https://www.conventionalcommits.org/):
* `feat:` Nuevo entorno de escritorio o guía principal (ej: `feat: add River compositor guide`)
* `fix:` Corrección de sintaxis o comando desactualizado (ej: `fix: update hyprland windowrulev2 syntax`)
* `docs:` Mejoras en la documentación
* `chore:` Tareas de mantenimiento del repositorio

### 3. Enviar el Pull Request
1. Asegúrate de que los enlaces Markdown relativos funcionen correctamente.
2. Sube tus cambios a GitHub (`git push origin feat/add-river-wm`).
3. Abre un Pull Request hacia la rama `main`.

---

## 💡 Agradecimientos

Esta skill se inspiró originalmente en la arquitectura de skills del proyecto [Omarchy](https://omarchy.org/). Buscamos mantener ese mismo estándar de excelencia y seguridad para toda la comunidad de Arch Linux.
