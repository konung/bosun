<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/images/bosun-mark-dark.svg">
  <img src="docs/images/bosun-mark-light.svg" alt="bosun: a lowercase b drawn as one rope, its end hooked back over the top and its bowl coiled flat" width="56">
</picture>

# bosun

On a ship, the captain decides where the ship goes, the first mate turns that into orders, and the
boatswain (the bosun) passes the orders to the crew and makes sure the work gets done. On your
computer, you are the captain, an AI agent can be your first mate, and bosun is the boatswain. You
know how to do everything bosun does; you just shouldn't have to do it by hand every time.

You can run bosun yourself, which is sometimes the best way, or teach your AI agent to use it and
let the agent drive. Every command answers in the same shape: a readable table for you, or JSON for
an agent or for another tool such as `jq`.

bosun is a rewrite and consolidation of scripts I have used for years, some for a single chore and
some for several, each with its own flags, its own output and its own way of getting credentials
into the environment. Here they become one tool with one way of doing things.

## Principles

bosun is opinionated. The choices come from years of my own experience, flaws included.

- **Convention over configuration.** Sensible defaults for everything, and a config file for when
  you want something different.
- **Ruby.** It answers in milliseconds, which is fast enough for anything a person or an agent is
  waiting on. It runs on every system I touch, it is easy to read and change, and I enjoy writing it.
- **dry-rb.** Commands, settings, validation and results are built on the
  [dry-rb](https://dry-rb.org) libraries, so the same code is not written twice.
- **Pluggable.** Each job lives in a plugin, so every project, machine or container gets only the
  plugins it needs. When bosun can't talk to something yet, a new plugin is easy to write.
- **Don't reinvent.** Use the Ruby ecosystem. Prefer pure Ruby, so nothing needs compiling, or call
  the system's own tools, and give every command the same input and output.

## What I use it for

- **Watch the network.** Check that the machines and services at home are reachable and healthy,
  and say plainly what isn't.
- **Handle downloads.** Hand a download to the machine that should fetch it and report how it is
  going. A hundred other tools can do this; I like doing it from the command line.
- **File things where they belong.** Move documents and notes into place according to filing
  rules. Mine are the defaults; yours can replace them.
- **Keep plans and notes with the project.** Update a project's roadmap and check off finished work
  in the project itself, so people and AI agents working on it find every plan in the same place.
  If you prefer a central notes system, link it to those locations.
- **Manage access to credentials in one place.** Give a chore the login it needs from your password
  manager at the moment it runs. You decide once, in an `.env` file, which credentials each chore
  may use; the file holds references to the password manager, never the secrets themselves.
- **Set up a new machine.** Show what is installed, configured and missing, and offer to fix what
  is missing.
- **Manage and monitor the NAS.** Through its API where there is one, over SSH where there isn't.
- **Manage the terminal.** Workspace and session configuration for cmux, tmux and iTerm2.
- **Monitor and update self-hosted services** such as Uptime Kuma and Calibre.

## Plugins

I am releasing my own tools as plugins as I move them into bosun, and anyone can write more. A
plugin is a Ruby gem named `bosun-*`, installed from RubyGems or straight from a git repository.

The contract is small, like orders on a ship: bosun hands a plugin a command in a known format and
expects a result in a known shape. How the plugin gets the job done is up to the plugin. It should
be easy to read, document itself, and provide its own help.

## Status

Early. The name is reserved on RubyGems, and the core that plugins build on is being written for
version 0.1.0. The uses above arrive as plugins after that.

## Built with

Ruby 3.3 or newer and the [dry-rb](https://dry-rb.org) libraries. Tests use Minitest. Plugins may
bring their own dependencies.

## Licence

MIT. See `LICENSE.txt`.
