# bosun

A boatswain for your machine: one command-line entry point for the chores you run by hand, extended
by `bosun-*` plugin gems.

## Status

Early. The gem name is reserved on RubyGems (0.0.1 prints its version and nothing more). Version
0.1.0 is being built here and will ship the core that plugins build on.

## What 0.1.0 will do

- **One entry point, nested commands.** `bosun network ping table`, not a separate script per chore.
  `bosun tree` shows every command, and `bosun completions fish|zsh|bash` prints shell completions.
- **Plugins are gems.** A plugin is an ordinary gem named `bosun-*`, installed from RubyGems or from
  a git repository, the way Bundler handles a git source:

  ```sh
  bosun plugins add bosun-network
  bosun plugins add https://example.com/you/bosun-something.git --branch main
  bosun plugins list
  ```

  Plugins live in their own Gemfile under `~/.config/bosun/plugins/`. A plugin that fails to load is
  reported and skipped; it never takes the rest of bosun down with it.
- **Lazy loading.** Listing commands, help and completions never load a plugin's implementation;
  only the command you run does.
- **Human or JSON output.** Every command answers in a readable table by default and in JSON with
  `-f json`. Exit codes: `0` ok, `1` findings, `2` refused.
- **Secrets stay out of files.** A plugin declares the environment variables it needs. bosun takes
  them from the environment, or resolves a reference such as `op://vault/item/field` through a
  secret-provider plugin at the moment the command runs. Nothing resolved is written to disk.
- **`bosun setup`** reports what is installed, configured and missing, and offers to set up what is
  missing. It stores references, never secret values.

## Built with

Ruby 3.2 or newer and the [dry-rb](https://dry-rb.org) libraries: dry-cli for commands, dry-system
for loading, dry-struct and dry-validation for data, dry-monads for results. Tests use Minitest.

## Licence

MIT. See `LICENSE.txt`.
