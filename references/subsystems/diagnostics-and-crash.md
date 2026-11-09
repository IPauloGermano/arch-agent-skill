# Crash Diagnosis and System Diagnostics (Arch Linux)

This guide provides deterministic, evidence-based methods for diagnosing application crashes, segfaults, core dumps, and system failures on Arch Linux using upstream systemd and GNU/Linux tooling.

---

## 1. Establishing Crash Facts (`coredumpctl`)

When an application crashes or disappears, start from evidence using `systemd-coredump`:

```bash
# List recent core dumps
coredumpctl list

# Inspect details of the most recent crash or specific PID
coredumpctl info
# or
coredumpctl info <pid_or_process_name>
```

### Critical Details to Note:
* **Executable & Command Line:** Reveals exact CLI flags, input files, or workspace path active when the crash occurred.
* **Signal:** Identifies whether it was a `SIGSEGV` (memory access violation), `SIGABRT` (assertion failure), `SIGFPE` (math error), or `SIGILL`.
* **Coredump Size & State:** Indicates if a usable core dump was captured and stored by systemd.

---

## 2. Rule Out Resource Exhaustion (OOM)

Before diagnosing code bugs, verify whether the kernel Out-Of-Memory (OOM) killer or systemd-oomd terminated the process:

```bash
# Check memory and swap pressure
free -h

# Check kernel logs for OOM kills in the current boot
dmesg -T | grep -i oom
# or
journalctl -k -b | grep -Ei "oom|out of memory|killed process"
```

> [!NOTE]
> A process killed by the kernel OOM killer or `systemd-oomd` is a victim of memory exhaustion, not necessarily a software defect in that specific process.

---

## 3. Correlate with System Timeline

Crash timestamps are essential diagnostic anchors:

1. **Recent Package Updates:**
   Check if the crashing package or its shared libraries (`glibc`, `mesa`, `qt6`, `gtk4`) were updated shortly before the crash:
   ```bash
   grep -E "upgraded|installed" /var/log/pacman.log | tail -n 50
   ```
2. **Journal Logs Around the Event:**
   View logs spanning 60 seconds before and after the crash:
   ```bash
   journalctl --user --since "2026-01-01 12:00:00" --until "2026-01-01 12:02:00" -e
   ```
3. **Recent File Modifications:**
   Inspect files recently written in `~/.config/<app>/` or the active working directory:
   ```bash
   find ~/.config/<app> -type f -mmin -60
   ```

---

## 4. Symbolizing Backtraces via Arch Linux Debuginfod

Arch Linux operates a public `debuginfod` service (`debuginfod.archlinux.org`), enabling on-demand debug symbol downloading without manually compiling debug packages.

### Secure Symbolization Recipe:

```bash
# 1. Allocate a secure, ephemeral core file
core=$(mktemp -t crash-XXXXXX.core)
trap 'rm -f "$core"' EXIT

# 2. Extract core dump from systemd-coredump
coredumpctl dump <pid> --output="$core"

# 3. Query GDB with Debuginfod enabled
DEBUGINFOD_URLS="https://debuginfod.archlinux.org" \
  gdb -q /usr/bin/<executable> "$core" \
  -batch \
  -ex 'set debuginfod enabled on' \
  -ex 'bt' \
  -ex 'thread apply all bt'

# 4. Core file is automatically removed by trap on script exit
```

> [!CAUTION]
> **Data Privacy Warning:** Core dumps contain a byte-for-byte copy of the process's RAM at the moment of death. This may include unencrypted passwords, API tokens, encryption keys, and private files.
> * Always store cores in private `mktemp` paths.
> * **Always delete cores immediately** after symbolization.
> * **Never** upload raw core files to public issue trackers or paste unredacted stack dumps containing credentials.

---

## 5. Identifying In-Process Third-Party Code

When analyzing backtraces, check whether foreign code is loaded into the address space:
* Browser or file-manager extensions (`nautilus-python`, Chrome/Firefox addons).
* Shell plugins or dynamic libraries.
* Out-of-tree or proprietary drivers (NVIDIA proprietary driver, custom audio modules).

If third-party libraries appear in the stack frames leading to the fault, test whether reproducing in a clean profile resolves the failure.

---

## 6. Diagnostic Reporting Guidelines for Agents

When presenting crash findings to the user:
1. **Identify the Process:** Program name, version (`pacman -Q <package>`), and exact arguments.
2. **Differentiate Fact from Inference:** Clearly separate what the symbolized stack **proves** from speculative mechanisms.
3. **Check Data Integrity:** Check `~/.local/share/Trash` or autosave files before declaring user data lost.
4. **Non-Destructive Principle:** Diagnostics should only inspect. Do not wipe user caches, delete configs, or reinstall packages unless explicitly instructed.
