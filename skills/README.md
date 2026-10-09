# Skills

Each skill is one directory containing a `SKILL.md`:

```text
skills/
└── <skill-name>/
    ├── SKILL.md        required: frontmatter + instructions
    ├── scripts/        optional: code the agent can run
    ├── references/     optional: docs the agent reads on demand
    └── assets/         optional: templates, images, data files
```

This is the [Agent Skills](https://agentskills.io) format. Claude, Codex,
Hermes and OpenClaw all read it, so one skill works in all four.

## Adding a skill

1. Copy the template:

   ```sh
   cp -r skills/example-skill skills/<skill-name>
   ```

2. Edit the frontmatter at the top of `SKILL.md`:

   ```yaml
   ---
   name: <skill-name>
   description: What the skill does and when the agent should use it.
   ---
   ```

   - `name` must match the directory name: lowercase letters, digits and
     single hyphens, at most 64 characters, not starting or ending with a
     hyphen.
   - `description` is what the agent sees when deciding whether to load the
     skill, so say both *what* it does and *when* to use it. Keep it under
     1024 characters and put the trigger words first.
   - Stick to `name` and `description` unless you need an optional field from
     the [specification](https://agentskills.io/specification). Some agents
     reject unknown keys.

3. Write the instructions below the frontmatter. Keep `SKILL.md` short and
   move long reference material into `references/`; agents load those files
   only when the instructions point to them.

4. Install it locally (see the top-level README) and try it in at least one
   agent before opening a pull request.

## No secrets

Never put API keys, tokens, passwords or private keys in a skill, including
in examples and scripts. Read them from environment variables at run time
and document the variable name instead. Every push is scanned and a finding
fails the build; see the top-level README.
