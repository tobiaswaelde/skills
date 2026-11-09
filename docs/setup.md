# Vollständiges Codex-Setup auf einem neuen Rechner

Diese Anleitung richtet das persönliche Setup aus diesem Repository auf Linux oder macOS ein. Unter Windows wird dieselbe Linux-Anleitung innerhalb von WSL2 verwendet. Alle Befehle werden im Terminal ausgeführt, sofern nicht ausdrücklich ein Codex-Chat genannt wird.

Die Einrichtung umfasst:

| Bestandteil | Zweck | Originalquelle |
| --- | --- | --- |
| Codex CLI | Agent, Plugin-Verwaltung und MCP-Konfiguration | [OpenAI: Installation](https://learn.chatgpt.com/docs/cli) |
| uv | Python installieren und Python-CLI-Tools verwalten | [Astral: Installation](https://docs.astral.sh/uv/getting-started/installation/) |
| Spec Kit / `specify` | Spezifikationen, Pläne und Aufgaben pro Projekt | [GitHub: Installation](https://github.github.io/spec-kit/installation.html) |
| RTK | Shell-Ausgaben für den Agenten komprimieren | [rtk-ai/rtk](https://github.com/rtk-ai/rtk#installation) |
| Context7 | Aktuelle Bibliotheksdokumentation über MCP abrufen | [Context7: Codex](https://context7.com/docs/clients/codex) |
| Personal Skills, Matt Pocock, Caveman | Persönliche und externe Workflows | [Marketplace-Verzeichnis](../marketplaces.json) |

Benötigt werden Internetzugang, lokale Administratorrechte für Systempakete, ein GitHub-Konto sowie ein Konto mit Codex-Zugang. Für Context7 erfolgt eine separate Anmeldung. Die Anleitung verwendet die eigenständigen Installer und einen entfernten Context7-MCP-Server; Node.js, npm, Rust und ein separat installiertes System-Python sind dafür nicht erforderlich. Projektabhängige Laufzeiten wie Node.js werden später nach den Vorgaben des jeweiligen Projekts eingerichtet.

## 1. Betriebssystem vorbereiten

Nur den zum Betriebssystem passenden Abschnitt ausführen.

### Ubuntu / Debian

```sh
sudo apt update
sudo apt install -y git curl ca-certificates tar unzip ripgrep jq gh
```

Falls `gh` in den Paketquellen der verwendeten Distribution fehlt, die [offizielle Linux-Installation der GitHub CLI](https://github.com/cli/cli/blob/trunk/docs/install_linux.md) verwenden.

### Fedora

```sh
sudo dnf install -y git curl ca-certificates tar unzip ripgrep jq gh
```

### Arch Linux / CachyOS

```sh
sudo pacman -Syu --needed git curl ca-certificates tar unzip ripgrep jq github-cli
```

### macOS

Zuerst die Command Line Tools installieren und den Installationsdialog abschließen:

```sh
xcode-select --install
```

Anschließend [Homebrew](https://brew.sh/) installieren:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Die vom Installer ausgegebenen „Next steps“ ausführen, damit `brew` dauerhaft im `PATH` liegt. Danach ein neues Terminal öffnen und die Werkzeuge installieren:

```sh
brew install git gh ripgrep jq
```

### Windows mit WSL2

In PowerShell **als Administrator** ausführen:

```powershell
wsl --install -d Ubuntu
```

Windows bei Aufforderung neu starten, Ubuntu öffnen und den Linux-Benutzer mit Passwort anlegen. Danach den Abschnitt **Ubuntu / Debian** und sämtliche weiteren Shell-Befehle in Ubuntu ausführen. Projekte unter `~/projects` im Linux-Dateisystem ablegen. Eine native Windows-Codex-Installation hat eine eigene Benutzerkonfiguration; die WSL-Konfiguration wird dadurch nicht automatisch übernommen.

Quelle: [Microsoft: WSL installieren](https://learn.microsoft.com/en-us/windows/wsl/install).

### Benutzerverzeichnis für CLI-Werkzeuge vorbereiten

Für Bash oder Zsh das Verzeichnis `~/.local/bin` dauerhaft zum `PATH` hinzufügen. Nur den passenden Block ausführen.

Bash, beispielsweise unter Ubuntu oder CachyOS:

```bash
mkdir -p "$HOME/.local/bin"
touch "$HOME/.bashrc"
grep -Fqx 'export PATH="$HOME/.local/bin:$PATH"' "$HOME/.bashrc" || \
  printf '\nexport PATH="$HOME/.local/bin:$PATH"\n' >> "$HOME/.bashrc"
export PATH="$HOME/.local/bin:$PATH"
```

Zsh, beispielsweise im macOS-Standardterminal:

```zsh
mkdir -p "$HOME/.local/bin"
touch "$HOME/.zshrc"
grep -Fqx 'export PATH="$HOME/.local/bin:$PATH"' "$HOME/.zshrc" || \
  printf '\nexport PATH="$HOME/.local/bin:$PATH"\n' >> "$HOME/.zshrc"
export PATH="$HOME/.local/bin:$PATH"
```

Bei einer anderen Shell deren Startdatei entsprechend ergänzen. Die folgenden Beispiele setzen Bash oder Zsh voraus.

## 2. Git und GitHub anmelden

Die Platzhalter durch den eigenen Namen und eine bei GitHub verifizierte E-Mail-Adresse oder die persönliche GitHub-Noreply-Adresse ersetzen:

```sh
git config --global user.name "DEIN NAME"
git config --global user.email "DEINE GITHUB-E-MAIL"
gh auth login --hostname github.com --git-protocol https --web
gh auth setup-git
gh auth status
```

Den Browser-Login abschließen. HTTPS ist für diese Anleitung ausreichend; ein SSH-Schlüssel ist nicht erforderlich. Für private GitLab-/Forgejo-Projekte später deren eigene Git-Zugangsdaten und gegebenenfalls die passenden Codex-Plugins einrichten.

Quelle: [GitHub CLI: Quickstart](https://docs.github.com/en/github-cli/github-cli/quickstart).

## 3. Codex installieren und anmelden

Den offiziellen eigenständigen Installer verwenden:

```sh
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

Die PATH-Hinweise des Installers beachten und bei Bedarf ein neues Terminal öffnen. Danach:

```sh
codex --version
codex login
codex login status
codex plugin --help
```

Den ChatGPT-Login im Browser abschließen. Ohne lokalen Browser alternativ `codex login --device-auth` verwenden. Falls `codex plugin` nicht verfügbar ist, Codex aktualisieren, bevor die Marketplaces eingerichtet werden.

Für eine grafische Oberfläche zusätzlich die [Codex-App](https://developers.openai.com/codex/app) nach den dortigen Plattformhinweisen installieren und anmelden. Die weitere Einrichtung erfolgt über die CLI. App und CLI verwenden bei gleichem Benutzer und gleichem `CODEX_HOME` dieselbe Konfiguration; diese Anleitung setzt das Standardverzeichnis `~/.codex` voraus.

Quellen: [OpenAI: Codex CLI](https://learn.chatgpt.com/docs/cli), [CLI-Befehle](https://learn.chatgpt.com/docs/cli/reference).

## 4. uv und Python installieren

[uv](https://docs.astral.sh/uv/) verwaltet Python und isolierte Installationen von Python-CLI-Tools. Zuerst uv installieren:

```sh
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Ein neues Terminal öffnen, dann Python für Spec Kit installieren und den Werkzeugpfad einrichten:

```sh
uv --version
uv python install 3.12
uv tool update-shell
```

Falls `uv tool update-shell` die Shell-Konfiguration geändert hat, erneut ein Terminal öffnen. Python 3.12 erfüllt die Spec-Kit-Voraussetzung Python 3.11 oder neuer. Die explizite Python-Auswahl im nächsten Schritt vermeidet die Abhängigkeit vom System-Python.

Quellen: [uv installieren](https://docs.astral.sh/uv/getting-started/installation/), [Python verwalten](https://docs.astral.sh/uv/guides/install-python/), [CLI-Tools verwalten](https://docs.astral.sh/uv/concepts/tools/).

## 5. Spec Kit installieren

[Spec Kit](https://github.com/github/spec-kit) wird über das CLI-Programm `specify` bedient. Das offiziell veröffentlichte Paket mit uv installieren:

```sh
uv tool install --python 3.12 specify-cli
specify version
specify check
```

`specify check` zeigt auch andere unterstützte Agenten an. Nicht installierte Agenten sind kein Grund, sie zusätzlich einzurichten; für dieses Setup muss Codex verfügbar sein.

Für einen reproduzierbaren, auf eine bestimmte Version festgelegten Aufbau stattdessen einen geprüften Release-Tag aus den [Spec-Kit-Releases](https://github.com/github/spec-kit/releases) verwenden. `vX.Y.Z` vor dem Ausführen ersetzen:

```sh
uv tool install --python 3.12 specify-cli --from 'git+https://github.com/github/spec-kit.git@vX.Y.Z'
```

Diese beiden Befehle sind alternative Installationswege. Nicht zusätzlich mit `pip` oder `pipx` eine zweite Kopie installieren.

Quelle: [Spec Kit: Installation](https://github.github.io/spec-kit/installation.html).

### Spec Kit pro Projekt aktivieren

Die globale CLI-Installation legt noch keine Projektdateien an. Für ein neues Projekt:

```sh
mkdir -p "$HOME/projects"
cd "$HOME/projects"
specify init mein-projekt --integration codex --script sh
cd mein-projekt
codex
```

In einem vorhandenen Projekt zuerst lokale Änderungen committen oder sichern, dann in dessen Verzeichnis ausführen:

```sh
specify init --here --integration codex --script sh
```

Die Rückfrage für ein nicht leeres Verzeichnis und anschließend den Git-Diff prüfen. In einem bereits eingerichteten Spec-Kit-Projekt stattdessen `specify integration install codex` verwenden, falls die Codex-Integration noch fehlt.

Die Integration legt Skills unter `.agents/skills/speckit-*/SKILL.md` an. Danach einen neuen Codex-Chat im Projekt starten. Die folgenden Aufrufe gehören **in den Codex-Chat**, einzeln und mit Prüfung der jeweiligen Ergebnisse:

```text
$speckit-constitution Projektgrundsätze für Qualität, Tests und Wartbarkeit festlegen.
$speckit-specify Gewünschtes Verhalten und Anforderungen beschreiben.
$speckit-plan Technische Umsetzung planen.
$speckit-tasks Umsetzung in überprüfbare Aufgaben zerlegen.
$speckit-implement
```

Spec Kit wird bewusst pro Projekt eingerichtet. Der persönliche `coding-workflow` bleibt für seine Freigaben und den Lieferprozess maßgeblich; Spec Kit ersetzt diese Regeln nicht.

Quelle: [Spec Kit: Codex-Integration](https://github.github.io/spec-kit/integrations/codex.html).

## 6. RTK installieren und für Codex aktivieren

[RTK](https://github.com/rtk-ai/rtk) komprimiert unterstützte Shell-Ausgaben, bevor der Agent sie liest. Das richtige Projekt ist **rtk-ai/rtk**.

```sh
curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh
rtk --version
rtk gain
rtk init -g --codex
```

Die Ausgabe von `rtk init` prüfen und angebotene Konfigurationsänderungen bestätigen. `-g --codex` richtet die globale Codex-Integration mit Hook und Anweisungen in `~/.codex` ein. Ohne `--codex` wird eine andere Agenten-Integration eingerichtet.

Danach Codex vollständig neu starten. Die Integration lässt sich anzeigen, ohne Änderungen zu schreiben:

```sh
rtk init -g --codex --show
```

In einem Codex-Chat innerhalb eines Git-Repositorys den Agenten `git status` im Terminal ausführen lassen. Anschließend im eigenen Terminal `rtk gain` prüfen. Ein manuell im eigenen Terminal ausgeführtes `git status` testet den Codex-Hook nicht. Die Integration betrifft Shell-Aufrufe; andere eingebaute Werkzeuge werden dadurch nicht automatisch komprimiert.

Quellen: [RTK: Installation und Quick Start](https://github.com/rtk-ai/rtk#quick-start), [Installer](https://github.com/rtk-ai/rtk/blob/master/install.sh).

## 7. Context7 per MCP und OAuth verbinden

[Context7](https://github.com/upstash/context7) liefert aktuelle Bibliotheksdokumentation. Dieses Setup verwendet wie der bestehende Rechner den entfernten MCP-Server mit OAuth:

```sh
codex mcp add context7 --url https://mcp.context7.com/mcp/oauth
codex mcp login context7
codex mcp get context7
codex mcp list
```

Die Context7-Anmeldung im Browser abschließen. Für diesen Weg ist keine lokale Context7-Installation und kein manuell verwalteter API-Key nötig. Der OAuth-Endpunkt endet auf **`/mcp/oauth`**, nicht auf `/mcp`.

Codex speichert den Server in `~/.codex/config.toml`. Der relevante Eintrag sieht so aus:

```toml
[mcp_servers.context7]
url = "https://mcp.context7.com/mcp/oauth"
```

Den Eintrag nicht zusätzlich manuell anlegen, wenn er bereits über die CLI registriert wurde. Bei vorhandenem Server zuerst `codex mcp get context7` prüfen und dessen Konfiguration bei Bedarf gezielt anpassen.

Nach einem neuen Codex-Chat beispielsweise fragen: „Zeige mir die aktuelle Vue-Dokumentation zu `watch`. Nutze Context7.“ Erwartet werden Aufrufe zum Auflösen der Bibliothek und Abrufen der Dokumentation. Der Eintrag in `codex mcp list` allein beweist noch keinen erfolgreichen Dokumentationsabruf.

Quellen: [Context7: OAuth](https://context7.com/docs/howto/oauth), [Context7 mit Codex](https://context7.com/docs/clients/codex), [OpenAI: MCP](https://learn.chatgpt.com/docs/extend/mcp).

## 8. Persönliche und externe Skills installieren

Alle drei Marketplaces werden separat registriert. Die Datei `marketplaces.json` ist ein Verzeichnis der Quellen und installiert deren Plugins nicht automatisch.

```sh
codex plugin marketplace add https://github.com/tobiaswaelde/skills.git --ref main
codex plugin add personal-skills@tobiaswaelde

codex plugin marketplace add https://github.com/mattpocock/skills.git
codex plugin add mattpocock-skills@mattpocock

codex plugin marketplace add https://github.com/JuliusBrussee/caveman.git
codex plugin add caveman@caveman

codex plugin marketplace list
codex plugin list
```

Quellen: [Personal Skills](https://github.com/tobiaswaelde/skills), [Matt Pocock Skills](https://github.com/mattpocock/skills), [Caveman](https://github.com/JuliusBrussee/caveman).

Das persönliche Plugin enthält die sieben in der [README](../README.md#personal-skills) beschriebenen Skills. Caveman Ultra benötigt für die Antwortformatierung kein Caveman-Cloud-Konto. Cloud-/Learn-Funktionen haben zusätzliche Voraussetzungen ihres Anbieters.

## 9. Globale Regeln einrichten

`~/.codex/AGENTS.md` wird separat pro Rechner gepflegt. Die Plugin-Installation synchronisiert diese Datei nicht. Nach der RTK-Einrichtung die Datei in einem Texteditor öffnen und die folgenden Regeln ergänzen. Bereits vorhandene Regeln und den von RTK angelegten Verweis auf `RTK.md` erhalten; keinen zweiten RTK-Verweis hinzufügen.

```markdown
# Global Codex Instructions

## Response style

- Always use `/caveman ultra` (the `ultracave` skill) by default in every session, unless I explicitly request another mode.
- Always keep final responses concise.
- Prefer bullet points over paragraphs.
- Be technical and direct. Avoid long-form explanations unless explicitly requested.
- Do not repeat my request or describe every intermediate step.
- After completing a task, use only the relevant sections: Changed, Tests, Notes. Omit empty sections; aim for 3–8 bullet points.

## Coding workflow

- Use the `coding-workflow` skill by default for coding project tasks in every project, regardless of repository host or issue tracker, unless I explicitly request a different workflow. Read and follow its instructions without requiring me to invoke it by name.
- Prefer native Codex question/selection controls for clarifications and approvals whenever an available tool supports that purpose in the current mode. Offer concise action-specific choices instead of requiring a typed "yes"; wait for an actually submitted answer before dependent work.

## Documentation

- Use Context7 for current library and framework documentation, API references, setup and configuration questions. Match the project's version when available.
```

Die Datei enthält nach `rtk init -g --codex` außerdem den RTK-Verweis, im Standardverzeichnis beispielsweise:

```text
@/home/DEIN-BENUTZER/.codex/RTK.md
```

Der tatsächliche absolute Pfad wird von RTK erzeugt und ist auf macOS typischerweise unter `/Users/DEIN-BENUTZER/.codex/RTK.md`. Den Beispielpfad nicht unverändert übernehmen und die von RTK verwaltete `RTK.md` nicht durch eigene Inhalte ersetzen.

## 10. Abschließende Prüfung

Ein neues Terminal öffnen und ausführen:

```sh
git --version
gh auth status
codex --version
codex login status
uv --version
uv python find 3.12
specify version
specify check
rtk --version
rtk init -g --codex --show
codex plugin marketplace list
codex plugin list
codex mcp list
```

Danach Codex neu starten und prüfen:

- Personal Skills, Matt Pocock und Caveman sind installiert und im neuen Chat verfügbar.
- `$coding-workflow` ist aufrufbar; die globalen Regeln aktivieren es für Coding-Aufgaben standardmäßig.
- `/caveman status` meldet Ultra-Modus nach Anwendung der globalen Anweisung.
- Eine Context7-Frage führt zu einem echten Dokumentationsabruf ohne Authentifizierungsfehler.
- Ein vom Agenten ausgeführter unterstützter Shell-Befehl erscheint in den RTK-Statistiken.
- In einem mit Spec Kit initialisierten Projekt ist `$speckit-specify` verfügbar.

Bei `command not found` zuerst `command -v codex uv specify rtk` und den `PATH` prüfen. Bei fehlenden Skills einen neuen Chat starten und nötigenfalls die App neu starten. Bei Context7-Anmeldefehlern `codex mcp login context7` erneut ausführen. GitHub-, Codex- und Context7-Anmeldungen sind unabhängig voneinander.

## 11. Updates auf jedem Rechner

Für die oben verwendeten eigenständigen Installer und die uv-Installation aus PyPI:

```sh
curl -fsSL https://chatgpt.com/codex/install.sh | sh
uv self update
uv tool upgrade specify-cli
curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh
rtk init -g --codex

codex plugin marketplace upgrade tobiaswaelde
codex plugin add personal-skills@tobiaswaelde
codex plugin marketplace upgrade mattpocock
codex plugin add mattpocock-skills@mattpocock
codex plugin marketplace upgrade caveman
codex plugin add caveman@caveman
```

Bei einer auf Git-Tag festgelegten Spec-Kit-Installation stattdessen den ausgewählten neuen Tag mit `uv tool install --force --python 3.12 specify-cli --from 'git+https://github.com/github/spec-kit.git@vX.Y.Z'` installieren. Homebrew-Installationen werden mit `brew upgrade` aktualisiert; Installationswege nicht mischen.

Die Spec-Kit-CLI und bereits erzeugte Projektdateien sind getrennt zu aktualisieren. Für bestehende Projekte den [Spec-Kit-Upgrade-Guide](https://github.github.io/spec-kit/upgrade.html) befolgen und Änderungen vor der Übernahme prüfen. Der entfernte Context7-Server benötigt kein lokales Paketupdate. Nach Plugin- oder Hook-Updates Codex neu starten und die Abschlussprüfung wiederholen.

## 12. Konfiguration auf weitere Rechner übertragen

Dieses Repository ist die Quelle für die persönlichen Skills. Für einen weiteren Rechner die Anleitung erneut durchlaufen und anschließend die eigenen globalen Regeln übernehmen. Ein Klon ist nur erforderlich, wenn Skills bearbeitet werden sollen:

```sh
mkdir -p "$HOME/projects"
git clone https://github.com/tobiaswaelde/skills.git "$HOME/projects/skills"
```

`AGENTS.md` und benötigte, nicht geheime Einstellungen gezielt übertragen. GitHub-, Codex- und MCP-Anmeldungen auf jedem Gerät neu durchführen. Keine Zugangsdaten oder vollständigen Credential-Verzeichnisse in dieses öffentliche Repository übernehmen. Projektdateien von Spec Kit werden über die jeweiligen Projekt-Repositories bereitgestellt; weitere MCP-Server und Dienstverbindungen separat nach Bedarf einrichten.
