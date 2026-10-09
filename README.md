# skills

A collection of AI agent skills, written once in the
[Agent Skills](https://agentskills.io) format and usable from Claude, Codex,
Hermes and OpenClaw.

A skill is a folder of instructions (and optionally scripts and reference
files) that an agent loads when a task calls for it.

## Layout

```text
.
├── skills/
│   ├── README.md           how to write and add a skill
│   └── <skill-name>/
│       └── SKILL.md        frontmatter (name, description) + instructions
├── .github/workflows/
│   └── gitleaks.yml        secret scan on every push and pull request
└── .pre-commit-config.yaml the same scan, run locally before each commit
```

`skills/example-skill/` is a minimal template. See
[`skills/README.md`](skills/README.md) to add your own.

## Installing a skill

Clone the repository once, then link the skills you want into each agent's
skills directory. Linking rather than copying means `git pull` updates them.

```sh
git clone https://github.com/natgreenwood/skills.git ~/src/skills
```

Restart the agent (or start a new session) after installing.

### Claude Code

Personal skills, available in every project:

```sh
mkdir -p ~/.claude/skills
ln -s ~/src/skills/skills/<skill-name> ~/.claude/skills/<skill-name>
```

For one project only, link into `<project>/.claude/skills/` instead. In the
Claude apps, zip the skill folder and upload it in the Skills settings.

### Codex

Codex reads `~/.agents/skills/`, and `.agents/skills/` inside a repository:

```sh
mkdir -p ~/.agents/skills
ln -s ~/src/skills/skills/<skill-name> ~/.agents/skills/<skill-name>
```

### Hermes

Install straight from GitHub:

```sh
hermes skills install natgreenwood/skills/skills/<skill-name>
```

Or link it into `~/.hermes/skills/`, or add `~/.agents/skills` to
`skills.external_dirs` in `~/.hermes/config.yaml` to share the Codex
directory.

### OpenClaw

OpenClaw also reads `~/.agents/skills/`, so the Codex link above covers it.
To install a copy instead:

```sh
openclaw skills install ~/src/skills/skills/<skill-name>
```

## No secrets

This repository is public. Never commit API keys, tokens, passwords, private
keys or any other credential, not even in an example, a test fixture or a
commit you plan to revert. Git history is permanent, and anything pushed
here should be treated as leaked and rotated.

Skills that need credentials should read them from environment variables at
run time and document the variable name.

Two checks enforce this:

- **CI.** [gitleaks](https://github.com/gitleaks/gitleaks) scans the new
  commits on every push and pull request and fails the build if it finds a
  secret. Run the workflow manually from the Actions tab to scan the full
  history.
- **Locally.** Install [pre-commit](https://pre-commit.com) and enable the
  hook so gitleaks checks staged changes before each commit:

  ```sh
  pre-commit install
  ```

If gitleaks flags something that is not a secret, add the fingerprint it
prints to a `.gitleaksignore` file in the same pull request and explain why
in the description.

## License

[GPL-3.0](LICENSE)
