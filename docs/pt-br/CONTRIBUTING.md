# Contribuindo para a Arch Linux Agent Skill

[English](../../CONTRIBUTING.md) | [Português](CONTRIBUTING.md) | [Español](../es/CONTRIBUTING.md)

Obrigado pelo interesse em contribuir com a **Arch Linux Multi-Environment Agent Skill**! 🎉

Este repositório fornece conhecimento aberto, padronizado e com limites claros de segurança para assistentes de IA (Gemini, Claude, Antigravity e Open Agent Skills) que gerenciam sistemas Arch Linux.

---

## 🌟 Onde Você Pode Ajudar

1. **Adicionar Novos Window Managers & Ambientes:**
   * Suporte para *River*, *bspwm*, *AwesomeWM*, *Cosmic Desktop*, *Qtile*, *XFCE*, etc.
2. **Atualizar Guias Existentes:**
   * Acompanhar mudanças e novas versões do Hyprland, Waybar, KWin ou Mutter.
3. **Melhorar Automações e Scripts:**
   * Serviços de usuário do systemd, automações udev e listeners IPC de compositores.
4. **Expandir Receitas de Temas:**
   * Modelos para paletas populares (Catppuccin, Tokyo Night, Nord, Gruvbox) em terminais, barras e apps GTK/Qt.

---

## 📐 Padrões para Skills de Agentes de IA

Skills de IA exigem documentação concisa, sem ambiguidades e determinística:

### 1. Regras de Segurança Não-Destrutivas
* **Espaço de usuário prioritário:** Instruir agentes a gravarem configurações estritamente em `~/.config/` ou `~/.local/bin/`.
* **Backups Automáticos:** Sempre incluir passos de backup (`cp arquivo arquivo.bak.$(date +%s)`) antes de alterações.
* **Separação de Privilégios do AUR:** Sempre enfatizar que helpers do AUR (`yay`, `paru`) **nunca devem ser executados com `sudo`**.

### 2. Pureza Upstream
* Evitar wrappers proprietários ou restritos a uma distribuição específica.
* Utilizar ferramentas padrão do Arch Linux (`pacman`, `systemd --user`, formatos nativos `.conf`, `.jsonc`, `.kdl`).

### 3. Blocos de Código Claros
* Fornecer comandos CLI e trechos de configuração verificados com marcação de sintaxe apropriada (ex: ````ini````, ````bash````, ````jsonc````, ````css````, ````kdl````).

---

## 📝 Modelo para Novos Window Managers / DEs

Ao adicionar um novo ambiente (ex: `river.md` ou `cosmic.md`), siga esta estrutura:

```markdown
# Guia [Nome do Ambiente] (Arch Linux)

Este guia cobre a configuração e gerenciamento do **[Nome]** no Arch Linux.

---

## 1. Localização dos Arquivos de Configuração
* Configuração principal: `~/.config/[app]/...`
* Sub-configurações modulares (se aplicável)

## 2. Monitores e Escala
* Como listar e configurar resoluções, taxas de atualização e escala.

## 3. Atalhos Principais e Regras de Janela
* Sintaxe para aplicativos, navegação e janelas flutuantes.

## 4. Inicialização de Daemons e Barra de Status
* Como iniciar barras (Waybar/Polybar), notificações e bloqueio de tela.

## 5. Validação e Recarregamento
* Comandos para recarregar sem reiniciar a sessão.
```

---

## 🛠️ Fluxo de Desenvolvimento e Envio

### 1. Fork & Branch
```bash
# Clone o seu fork
git clone https://github.com/SEU_USUARIO/arch-linux-skill.git
cd arch-linux-skill

# Crie uma branch para a funcionalidade
git checkout -b feat/add-river-wm
```

### 2. Padrão de Commits
Seguimos o padrão [Conventional Commits](https://www.conventionalcommits.org/):
* `feat:` Novo ambiente desktop ou guia principal (ex: `feat: add River compositor guide`)
* `fix:` Correção de sintaxe ou comando desatualizado (ex: `fix: update hyprland windowrulev2 syntax`)
* `docs:` Melhorias na documentação
* `chore:` Tarefas de manutenção do repositório

### 3. Abrindo o Pull Request
1. Verifique se os links Markdown estão corretos.
2. Envie suas alterações (`git push origin feat/add-river-wm`).
3. Abra um Pull Request contra a branch `main`.

---

## 💡 Agradecimentos

Esta skill foi originalmente inspirada na arquitetura de skills do projeto [Omarchy](https://omarchy.org/). Buscamos manter esse mesmo padrão de excelência e segurança para toda a comunidade Arch Linux.
