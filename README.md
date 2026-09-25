<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/images/bosun-mark-dark.svg">
  <img src="docs/images/bosun-mark-light.svg" alt="bosun: a lowercase b drawn as one rope, its end hooked back over the top and its bowl coiled flat" width="56">
</picture>

# bosun

![A three-panel comic. A polar bear bosun in a pea coat sounds a horn from the deck of a small boat; three penguin sailors in striped shirts answer "Aye, bosun!" beside coiled rope and parcels; a penguin hands the bosun a logbook and reports "All done! 3 filed, 1 fetched." under a flag showing the bosun mark.](docs/images/bosun-comic.png)

On a ship, the captain decides where the ship goes, the first mate turns that into orders, and the
boatswain (the bosun) passes the orders to the crew and makes sure the work gets done. On your
computer, you are the captain, an AI agent is your first mate, and bosun is the boatswain. You
know how to do everything bosun does; you just shouldn't have to do it by hand every time.

You can run bosun yourself, which is sometimes the best way, or teach your AI agent to use it and
let the agent drive. Every command answers in the same shape: a readable table for you, or JSON for
an agent or for another tool such as [`jq`](https://jqlang.org).

That also saves an agent's tokens. Without bosun, an agent works out each chore from scratch:
it explores the system, tries commands and reads pages of their output. With bosun it runs one
command it already knows and gets back a short, structured answer, so its effort goes into deciding
what to do rather than rediscovering how.

bosun is a rewrite and consolidation of scripts I have used for years, some for a single chore and
some for several, each with its own flags, its own output and its own way of getting credentials
into the environment. Here they become one tool with one way of doing things.

## Principles

bosun is opinionated. The choices come from years of my own experience, flaws included.

- **[Convention over configuration](https://en.wikipedia.org/wiki/Convention_over_configuration).**
  Sensible defaults for everything, and a config file for when you want something different. A
  habit from years of [Rails](https://rubyonrails.org/doctrine).
- **[Ruby](https://www.ruby-lang.org).** Fast enough for anything a person or an agent is waiting
  on, and [getting faster](https://github.com/matz/spinel#computation). It runs on every system I
  touch, it is easy to read and change, and it
  [is designed to make programmers happy](https://www.ruby-lang.org/en/about/). It works on me.
- **dry-rb.** Commands, settings, validation and results are built on the
  [dry-rb](https://dry-rb.org) libraries, an excellent set of small, focused gems that keep bosun
  [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself).
- **Loaded the Hanami way.** bosun's container is [dry-system](https://dry-rb.org/gems/dry-system/),
  the one [Hanami](https://hanamirb.org) is built on, and it follows Hanami's approach to loading:
  the app is [prepared, not booted](https://guides.hanamirb.org/v2.2/app/container-and-components/),
  so each component loads the first time it is used. Listing commands, showing help and completing
  commands never load a plugin's code, so bosun starts in milliseconds however many plugins are
  installed. [Zeitwerk](https://github.com/fxn/zeitwerk) finds the files.
- **Pluggable.** Each job lives in a plugin, so every project, machine or container gets only the
  plugins it needs. When bosun can't talk to something yet, a new plugin is easy to write.
- **Don't reinvent.** Use the Ruby ecosystem. Prefer pure Ruby, so nothing needs compiling, or call
  the system's own tools, and give every command the same input, output and format, which is what
  keeps bosun predictable.

## What I use it for

- **Watch the network.** Check that the machines and services at home are reachable and healthy,
  and say plainly what isn't, in output that is pleasant to read.
- **Handle downloads.** Hand a download to the machine that should fetch it and report how it is
  going. A hundred other tools can do this; I like doing it from the command line.
- **File things where they belong.** Move documents and notes into place according to filing
  rules. The defaults are my own rules, shaped by how my brain works, not a claim that they are
  the best way; replace them with yours.
- **Keep plans and notes with the project.** Update a project's roadmap and check off finished work
  in the project itself, so people and AI agents working on it find every plan in the same place.
  If you prefer a central notes system, point it at those locations: I simply link
  [Obsidian](https://obsidian.md) to them, and a `bosun-notes-obsidian` plugin will do that for
  you.
- **Manage access to credentials in one place.** Give each sailor (a plugin) the login it needs
  from your password manager at the moment it runs. You decide once, in a
  [dotenv](https://github.com/bkeepers/dotenv) file, which credentials each chore may use. The file
  holds references to the password manager rather than the secrets themselves, unless you choose
  to put a value there, an approach borrowed from
  [Kamal's secrets](https://kamal-deploy.org/docs/configuration/environment-variables/#secrets).
- **Set up a new machine.** Show what is installed, configured and missing, and offer to fix what
  is missing, following my preferences by default and yours once you change them.
- **Manage and monitor the NAS.** A Synology NAS, through its API where there is one and over SSH
  where there isn't.
- **Manage the terminal.** Workspace and session configuration for [cmux](https://cmux.dev),
  [tmux](https://github.com/tmux/tmux) and [iTerm2](https://iterm2.com).
- **Monitor and update self-hosted services** such as [Uptime Kuma](https://uptime.kuma.pet) and
  [Calibre](https://calibre-ebook.com).
- **Whatever else you need.** If it's a chore you repeat, it can become a plugin.

## Plugins

I am releasing my own tools as plugins as I move them into bosun, and anyone can write more. A
plugin, one sailor in the crew, is a Ruby gem named `bosun-*`, installed from
[RubyGems](https://rubygems.org) or straight from a git repository, the way
[Bundler installs gems from git](https://bundler.io/guides/git.html).

The contract between bosun and a plugin is small, like orders on a ship: bosun passes the order
to the crew in a known format and expects the answer back in a known shape. How the plugin gets the
job done is up to the plugin. It should be
[self-documenting code](https://en.wikipedia.org/wiki/Self-documenting_code), easy to read, and
provide its own help.

## A sharp tool - [DANGER, DANGER](https://www.youtube.com/watch?v=R-FxmoVM7X4)

bosun runs real commands on real machines: it moves files, changes configuration and talks to your
services with your credentials. **THAT IS THE ENTIRE POINT OF IT**, and it is also why it deserves care. **Use it at your own risk, and especially
when an AI agent is driving it: give the agent guardrails, such as which commands it may run and
which machines it may touch, before you hand it the wheel.**

## Status

Early. The name is reserved on RubyGems, and the core that plugins build on is being written for
version 0.1.0. The uses above arrive as plugins after that.

## The name

Container tools already took their names from the waterfront: Docker from the dock worker, and
[Kubernetes](https://kubernetes.io/docs/concepts/overview/) from the Greek for helmsman. bosun
hands out work to the sailors on a ship, so it's named for the crew member who does that. Also, I
like sailboats.

## Built with

Ruby 3.3 or newer and the [dry-rb](https://dry-rb.org) libraries. Tests use [Minitest](https://github.com/minitest/minitest). Plugins may
bring their own dependencies.

## Licence

MIT. See `LICENSE.txt`.
