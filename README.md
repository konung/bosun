# bosun

On a ship, the boatswain keeps the rigging, the deck and the gear in working order, so the rest of
the crew can get on with sailing. bosun does the same for the machines and the network at home:
it can take care of the recurring chores, so you don't have to remember how each one is done.

bosun is a rewrite and consolidation of scripts I have used for years, each written for one chore,
with its own flags, its own output and its own way of getting a password. Here they become one
tool with one way of doing things.

## What it can do

- **Watch the network.** Check that the machines and services at home are reachable and healthy,
  and say plainly what isn't.
- **Handle downloads.** Hand a download to the machine that should fetch it and report how it is
  going.
- **File things where they belong.** Move documents and notes into place according to your own
  filing rules.
- **Keep plans and notes current.** Update project roadmaps and check off finished work.
- **Manage access to credentials in one place.** Give a chore the login it needs from your password
  manager at the moment it runs. You decide once which credentials each chore may use; nothing is
  copied into scripts or config files.
- **Set up a new machine.** Show what is installed, configured and missing, and offer to fix what
  is missing.
- **Grow with you.** Take on a new chore as a plugin, whether you write it or someone shares it,
  without changing bosun itself.

## How it works

Every chore is a `bosun` command. Plugins are Ruby gems installed from RubyGems or from a git
repository. Results print as readable tables, or as JSON for other scripts.

## Status

Early. The name is reserved on RubyGems, and the core that the chores build on is being written for
version 0.1.0. The chores above arrive as plugins after that.

## Built with

Ruby 3.2 or newer and the [dry-rb](https://dry-rb.org) libraries. Tests use Minitest.

## Licence

MIT. See `LICENSE.txt`.
