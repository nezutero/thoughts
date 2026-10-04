---
title: "Nothing to Fear: Self-Hosting on NixOS"
date: 2026-08-26
tags: ["NixOS", "Linux", "Self-Hosting"]
---

*This is Part 3 of my Linux Odyssey series. [Part 1](https://nezutero.dev/my-linux-odyssey-how-i-ended-up-on-nixos/) covers how I ended up on NixOS in the first place, and [Part 2](https://nezutero.dev/almost-went-back-one-month-on-nixos/) covers my first month actually living on it, flash drive and all. If you haven't read them, start there.*

Here I'll be talking about modularizing my NixOS config, merging two repos into one, and finally self-hosting again without the dread.

> The only thing we have to fear is fear itself.
>
> -- Franklin D. Roosevelt

The title isn't subtle, I know. But four self-hosting attempts in, I think I've earned the reference.

---

## A Quiet Month

Nothing dramatic happened this past month, and honestly, that's kind of the point. No crisis of confidence, no Arch ISO on standby, no long Discord threads trying to figure out if I was doing this whole thing wrong. The hybrid approach I landed on last time -- Nix for anything with decent module support, native configs symlinked in through home-manager for everything else -- just held. I made small changes here and there, nothing worth a post on its own.

What I didn't expect was for that quiet stability to be exactly what pulled me back toward something I'd more or less given up on: self-hosting.

---

## The Itch Comes Back

If you read Part 1, you might remember the side story buried in there: renting a Hetzner box, setting up Caddy, throwing Immich, Uptime Kuma, Nextcloud, Gitea, and Vaultwarden into a pile of Docker Compose files, and abandoning the whole thing a week later because I realized I didn't actually need any of it -- I just wanted to tinker. I tried again a few months after, lost my configs because I hadn't backed anything up, tried a third time after that, and got buried in firewall conflicts and Docker quirks I'd apparently solved before but never written down anywhere.

That third failure is basically the seed this entire series grew out of. It's the same frustration that made me think, for the first time, about NixOS as an actual answer rather than just something my friend kept mentioning.

So a couple of weeks ago, the itch came back: the urge to have my own little corner of infrastructure instead of handing everything to someone else's cloud. This time, though, there was no dread attached to it -- no mental tax of "if I mess this up I'll be redoing all of this from memory in three months." I just thought, sure, let's do it, I'll do it once and it'll stay there forever.

That's the whole difference. Self-hosting kept failing for me because of exactly the class of problem NixOS exists to solve: configuration I couldn't reconstruct, steps I forgot to document, a system that quietly drifted the moment I stopped paying attention to it. This time I have nothing to fear. I use NixOS. It's the same logic behind most of what I've been trying to fix in my life lately, honestly -- change the structure so the discipline isn't the thing holding it up anymore.

---

## Keeping It Small, On Purpose

Last time I self-hosted, I went in with five services on day one and burned out by day seven -- so this time I made myself pick only the things I'd actually use, not the things that sounded fun to set up.

I landed on two: Vaultwarden, because I'm not content with unix-pass used via rofi (too clunky, no browser integration, lacking some functionality), and Immich, because I don't trust a third party with the only copies of years of photos (that's why I've just been storing them locally, but I think it's time to finally back them up before I lose them one day). That's the whole list.

I'm fairly confident it'll grow. Uptime Kuma is an obvious next candidate, and I could see myself wanting some kind of git forge again eventually. But I'm deliberately not planning that out in advance this time -- I'll add things when I actually reach for them, not because a homelab checklist somewhere told me I should be running twelve services.

---

## One Repo, Not Two

The bigger structural change, and the thing I actually want to write about here, is what happened to my config layout once a second machine entered the picture.

By the end of Part 2 I'd settled into two separate repos: `~/nixos` for anything genuinely Nix (flakes, system config, home-manager wiring), and `~/dotfiles` for native-format application configs that home-manager symlinked into place. That worked fine for one machine.

The moment I needed a server, the split stopped making sense. A server doesn't have Hyprland or Waybar theming to symlink, but it does need to share a flake and a set of common system modules with my desktop -- and the same overall philosophy. Keeping two repos would've meant either duplicating boilerplate across both or awkwardly reaching between them, and I didn't want to maintain either option.

So I merged them. Everything now lives in one repo, and since `~/dotfiles` was already the one I'd been linking in these posts, I folded the old `~/nixos` content into it rather than the other way around. One flake, one lockfile, one place to look when something breaks. In hindsight it feels like an obvious move, and in my humble opinion I don't think I'd have arrived at it without a second host forcing the question.

---

## Modularizing, Naturally

Once everything lived in one place, the flat structure from before stopped holding up too -- not because it was wrong, exactly, but because a single set of top-level files stops making sense once you're describing more than one machine. So I split things up, not by sitting down to architect a module system in the abstract, but by actually needing somewhere for the server's config to live that wasn't the desktop's config, and going from there. Here's where I landed:

```
.
├── flake.lock
├── flake.nix
├── hosts
│   ├── default
│   │   ├── configuration.nix
│   │   ├── hardware-configuration.nix
│   │   ├── home.nix
│   │   └── networking.nix
│   └── server
│       └── configuration.nix
├── init.sh
├── modules
│   ├── home
│   │   ├── default.nix
│   │   ├── dunst.nix
│   │   ├── fastfetch.nix
│   │   ├── foot.nix
│   │   ├── git.nix
│   │   ├── gtk.nix
│   │   ├── hyprland
│   │   │   ├── config
│   │   │   │   ├── hypridle.conf
│   │   │   │   ├── hyprland.conf
│   │   │   │   ├── hyprlock.conf
│   │   │   │   └── hyprpaper.conf
│   │   │   └── default.nix
│   │   ├── mpd.nix
│   │   ├── nvim
│   │   │   ├── config
│   │   │   │   ├── init.lua
│   │   │   │   ├── lazy-lock.json
│   │   │   │   └── lua
│   │   │   └── default.nix
│   │   ├── rofi
│   │   │   ├── default.nix
│   │   │   └── theme.rasi
│   │   ├── scripts.nix
│   │   ├── shell.nix
│   │   ├── ssh.nix
│   │   ├── tmux.nix
│   │   ├── waybar
│   │   │   ├── default.nix
│   │   │   └── style.css
│   │   ├── xdg.nix
│   │   ├── yazi.nix
│   │   ├── zathura.nix
│   │   └── zen-browser.nix
│   └── nixos
│       ├── fonts.nix
│       ├── maintenance.nix
│       ├── nextdns.nix
│       ├── nix.nix
│       ├── services.nix
│       └── tmux.nix
├── profiles
│   ├── desktop.nix
│   └── server.nix
├── scripts
│   ├── backlight.sh
│   ├── battery_notify.sh
│   ├── fzfman.sh
│   ├── manga_prep.sh
│   ├── rofipass.sh
│   ├── rofi-power-menu.sh
│   ├── temperature.sh
│   ├── volume.sh
│   ├── youtube-mpv.sh
│   └── zathura_rofi.sh
└── secrets
    └── nextdns.yaml
```

`hosts/` is the entry point per machine: `default` is my desktop, `server` is the new box. Each one stays as thin as I can manage, ideally just hardware specifics, networking, and a single import of whichever profile applies.

That's what `profiles/` is for. Instead of every host importing a long list of individual modules directly, `desktop.nix` and `server.nix` each bundle up the set of `modules/home` and `modules/nixos` entries that make sense for that kind of machine. My desktop profile pulls in Hyprland, Waybar, fonts, all of it. My server profile pulls in a small handful of `modules/nixos` entries and nothing from `modules/home` at all, because there's no home-manager story on a headless box with no user-facing applications.

`modules/` itself splits the way it conceptually always did -- home-manager stuff versus system-level stuff -- except both now live under one roof instead of two separate repos. The real change from Part 2 is how I handle apps that need a native config file. Back then, anything with a substantial native format lived in a completely separate top-level dotfiles folder, split away from its Nix wiring. Now each of those apps gets its own module directory, with the native config nested right inside it: `modules/home/hyprland/config/hyprland.conf` sits next to `modules/home/hyprland/default.nix`, same for `nvim` and `waybar` and `rofi`. The Nix file and the format it manages live in the same place. It's a small thing, but it means I'm not jumping between two folders to understand one app's setup anymore, which is exactly the kind of friction I was trying to get rid of.

Everything simpler than that -- `git`, `ssh`, `shell`, `tmux`, `gtk`, and the rest -- stays a single flat `.nix` file, because those have good enough module support that there's no native format worth embedding at all.

`scripts/` stays outside `modules/` on purpose. These are plain shell scripts my rice depends on -- backlight control, a rofi power menu, a couple of fzf and zathura helpers -- and `modules/home/scripts.nix` is just the thin bit of Nix that gets them onto my `PATH`. No reason to wrap them in Nix beyond that.

`secrets/` is new, obviously, because self-hosting anything real means real credentials. Right now it's just the NextDNS config, which needs an actual profile ID that has no business sitting in plaintext in a repo I link publicly from this blog. It's encrypted using `sops-nix`, rather than gitignored, which was the whole point of putting it in the repo at all instead of keeping it somewhere separate that I'd inevitably forget to back up.

---

## The Server

`hosts/server` is deliberately boring. No home-manager, no GUI, just the server profile and whatever `modules/nixos/services.nix` declares. Vaultwarden and Immich are both configured through their native NixOS service modules, `services.vaultwarden` and `services.immich` -- not a single Docker container or Compose file in sight.

That last part is the one that actually gets me. My first attempt at self-hosting was Debian plus Docker plus a pile of YAML I don't have anymore. My second and third attempts were the same thing again, minus whatever configs I'd lost in between. This time it's a couple dozen lines in `services.nix`, checked into the same repo as everything else, rebuildable on a fresh box in about the time it takes to run `nixos-rebuild switch`. Even if the server dies tomorrow, I'm not searching my phone for firewall rules I half-remember. I'm running one command. Amazing. Living the dream.

---

## What's Next

I don't have a grand roadmap here, and I'm trying to keep it that way on purpose given how the last few self-hosting attempts went. Vaultwarden and Immich are running, the repo is one clean tree instead of two, and for the first time trying this, none of it feels fragile.

I'll probably add something else eventually. When I do, it'll be because I actually wanted it, not because a list said I should, and it'll go into the same repo, in the same shape as everything else. That's kind of the whole point.

My dotfiles: <https://github.com/nezutero/dotfiles>

Thank you for reading.
