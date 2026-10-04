---
title: "Almost Went Back: One Month on NixOS"
date: "2026-07-21"
tags: ["NixOS", "Linux"]
---

*This is Part 2 of my Linux Odyssey series. [Part 1](https://nezutero.dev/my-linux-odyssey-how-i-ended-up-on-nixos/) covers my path from Ubuntu through Arch Linux and why I decided to try NixOS in the first place. If you haven't read it, start there.*

---

## The Breaking Point

About a week after I published my first post, I had a crisis of confidence.

I had been using NixOS for roughly four weeks at that point, and I was deep in it: learning about flakes, home-manager, the differences between various configuration approaches, the philosophy behind the whole project. I had hit several footguns, sat through long debugging sessions, and felt the weight of having an infinite number of ways to do any given thing. Instead of feeling like I was mastering something, I felt overwhelmed.

I had a prepared Arch Linux ISO on a flash drive. I was seriously considering going back.

---

## What I Was Missing (Kind Of)

The main thing pulling me back was not the complexity itself. Complexity I can deal with; I went through something similar when I first switched to Arch. It was something more subtle.

I missed the way I used to interact with my system.

On Arch, I had spent years building up a kind of physical intuition about Linux: knowing where configuration files live, understanding what they do, navigating the file system and editing things directly. That knowledge is universal across Unix systems that follow the FHS convention. It feels like real, transferable understanding. On NixOS, the need for most of it simply disappears. You do not go to `/etc/resolv.conf` to set your DNS; you declare it in a Nix file and NixOS generates the file for you. The abstraction handles it.

At first I found this liberating. After four weeks, I found it unsettling. I started to wonder whether I was actually learning Linux anymore, or just learning Nix. The two felt like different things, and I was not sure the latter was worth trading the former for.

I also worried that NixOS would limit my growth and understanding of the underlying system. On Arch, you had to do everything yourself, which meant learning where things are and why they are there. In NixOS, you learn how to interact with the system through the Nix abstraction, which distances you from the classical interaction model. I felt that distance after four weeks. I could do anything I wanted, but it felt less real somehow.

There is a genuine question lurking here. Does abstracting away from the system prevent you from understanding it? One argument against abstraction is exactly that: it hides the thing it controls, and the knowledge you would have built by interacting with it directly never gets built. On the other hand, that same knowledge becomes partly irrelevant every few years as conventions shift. Does memorizing config file locations and doing things imperatively actually deepen understanding, or is it just familiarity with a particular set of conventions that will eventually change anyway, making the acquired knowledge irrelevant?

I cannot deny that the imperative model I used to deal with is responsible for building intuition and understanding about why things work the way they do, how they interact, and how and why they are related to each other.

And yet the question arises: do I need to know everything and manually track so many things, including small, relatively unimportant ones? It is useful in some ways, yet useless in others. There are things I do not want to spend my time on; I just want them to work. At the same time, I want to understand how and why they work. I do not want to live in ignorance of my precious tool, especially if I want to master it.

I did not have a clean answer. What I had was doubt, a flash drive, and the NixOS Discord server.

---

## The Community

Rather than just switching back, I decided to post in the NixOS Discord server and hear what experienced users thought. I wrote out where I was:

> Hello, I've been using nix for 3 weeks, switched from arch which i had been using for 3.5 years.
>
> Tbh, these 3 weeks have been amazing so far: learned a lot of stuff, which is nothing yet, but feels great anyway. After using a completely different approach to defining my system, i've been both amazed and sad.
>
> I feel sad because although the nix approach is cool, functional, atomic, defined state, do it one time and it works forever (ideally, if nothing changes in NixOS), yet this led me to less interaction with the system, almost at all, and just "controlling" it via nix files, and while definiteness and declarativeness is THE way imho; after discovering the nix way, the imperative approach seems as stupid as writing commands one by one instead of creating a reusable .sh file.
>
> I just cannot see myself forgetting the stuff I have done and losing it, or fixing an issue and after reinstalling the system for whatever reason, and refixing the same thing again after forgetting it was there at all.
>
> I've been thinking about moving back to arch recently, I miss the way I used to explore different things about the system, interacting and changing everything I can think of, and learning/understanding FHS and stuff. I mean after years of using arch I memorised locations of different configs and files and what they do, as well as built some kind of intuition, which is almost always useless on NixOS.
>
> At the same time, I'm a big fan of NixOS and functional correctness. Also, compatibility issues caused by not having FHS... lsp servers in particular, damn it.......
>
> Anyway, I'm confused and idk what to do now, I've read a lot of stuff about this "dilemma", I mean should I switch to a distro with FHS (back to arch) or not.
>
> Looking for personal opinions from people who had a similar journey as mine.
>
> Just for info, I use hyprland, pretty much everything is used via terminal, configured and customised, I like control and knowing what and why is in my system.

I received a response from [mightyiam](https://github.com/mightyiam), author of the Full Time Nix podcast and a very active member of the Nix community.

I explained the LSP issue I had run into with clangd and Neovim due to the lack of FHS, which had taken hours to even begin to understand. I admitted that I had gone in with idealistic expectations and that the reality check was something I genuinely appreciated. And I asked him four questions:

1. How long he had been using NixOS
2. How he had discovered it
3. Why he had switched from classic distros
4. Why he kept choosing it over something like Arch

His answers:

> 1. I think it was home-manager before NixOS but it's July 2021: https://github.com/mightyiam/infra
> 2. https://github.com/gomain told me about it in person
> 3. hrm... that's a lot. hey, there's this interview of me if you're interested 🤷 https://youtu.be/0Jij9c9TdWo
> 4. because I get to keep it, share it across multiple computers, roll back, have finite control over any component, at any depth, contributing is accessible... cache is helpful...

Another community member, [EpicEric](https://github.com/EpicEric), also replied:

> I'm not that experienced but
>
> 1. Since December 2025
> 2. I'd heard about it before, I think I tried to learn it after a post from cafkafk but gave up. After a while I saw some videos from Vimjoyer and tonybtw and decided to give it another go
> 3. I tried it on my laptop first but quickly moved to having it installed on my whole fleet, including my desktop and servers. I was frustrated with how stagnant my usual distros were (Pop!_OS, Fedora), and how often stuff like Arch would break
> 4. NixOS adds its own set of challenges which are undeniably cumbersome, but I like the ease of upgrades and fresh updates, as well as a single git-tracked config for everything. I also like the fact that nixpkgs is open-source and easily inspectable, it lets me be more in control of my system

Reading those responses, something clicked. Point four from mightyiam and point three from EpicEric in particular: "I get to keep it" and "I was frustrated with how often stuff like Arch would break." That is exactly what I had been frustrated about losing with Arch, and exactly what NixOS gives you. The argument for staying was already sitting inside the argument I had been making for leaving.

I put the flash drive in a drawer.

---

## Your Linux Knowledge Actually Does Apply

One thing I kept reading before switching to NixOS was that your existing Linux knowledge basically does not carry over. You start from scratch, as a complete beginner, in a completely different paradigm.

After a month of using it, I think this is only partly true, and the part that is false matters a lot.

Yes, the interaction model is different. Yes, the mental model is different. And yes, you will feel like a beginner at first, because in many ways you are. But the underlying knowledge, what options mean, how services work, what a given configuration actually controls, transfers almost completely. When I search for a NixOS option for something I used to configure imperatively, I usually recognize what it does immediately, because I configured it before. The difference is in syntax and paradigm, not in substance.

What I had been calling "Linux knowledge" was actually two different things bundled together: understanding of how systems work, and familiarity with where things live on a particular filesystem. The first transfers perfectly to NixOS. The second becomes largely irrelevant, and whilst that felt like a loss at first, I now think it is fine. Knowing that configuration files live under `/etc` is useful context, but it is not deep understanding. Deep understanding is knowing what the configuration does and why, and that knowledge is entirely portable.

If anything, having a solid Arch background makes NixOS easier to learn, not harder. You already have the mental model of what you are trying to build; you just have to learn a new way to express it. The paradigm is new. The substance is not.

---

## Going All-In (and Learning Why Not To)

Having decided to stay, I committed fully. I wanted to do everything the Nix way: every application configured through Nix modules, everything modular, fully declarative top to bottom. I ended up with a config structure that looked like this:

```
.
├── flake.lock
├── flake.nix
├── hosts
│   └── default
│       ├── configuration.nix
│       └── hardware-configuration.nix
├── init.sh
└── modules
    ├── home
    │   ├── default.nix
    │   ├── dunst.nix
    │   ├── git.nix
    │   ├── gtk.nix
    │   ├── hyprland
    │   │   ├── default.nix
    │   │   ├── hypridle.nix
    │   │   ├── hyprland.conf
    │   │   ├── hyprland.nix
    │   │   ├── hyprlock.nix
    │   │   └── hyprpaper.nix
    │   ├── kitty.nix
    │   ├── niri.nix
    │   ├── nvim
    │   │   ├── default.nix
    │   │   ├── keymaps.nix
    │   │   ├── lsp.nix
    │   │   ├── options.nix
    │   │   ├── plugins.nix
    │   │   └── telescope.nix
    │   ├── rmpc.nix
    │   ├── rofi.nix
    │   ├── scripts
    │   │   └── ...
    │   ├── shell.nix
    │   ├── tmux.nix
    │   ├── waybar
    │   │   ├── config.nix
    │   │   ├── default.nix
    │   │   └── style.nix
    │   └── ...
    └── nixos
        ├── default.nix
        ├── packages.nix
        ├── services.nix
        └── system.nix
```

It got exhausting quickly. Some applications have excellent Nix support; configuring Neovim with NVF and Hyprland through its dedicated Nix modules was genuinely pleasant. But plenty of tools do not, and for those you end up generating configuration files by embedding them as raw strings inside your Nix config. It works, but it is messy, and it adds a layer of metaconfiguration that solves no real problem. You are not gaining anything over just writing the config file directly; you are just making it harder to read and maintain.

I quickly realized I had created a new kind of complexity that I had not intended. A new layer of abstraction over the abstraction.

I took this to the Discord server too:

> Hello, I'm curious how do you handle your nix configs, I mean do you do everything the "nix" way or not.
>
> I've tried modularizing my nix config and doing everything the nix way -- it felt exhausting; it became too complex and ugly: some apps don't support the nix way of doing things, thus I had to generate files from strings in nix configs, which felt ugly and messy, that's why I came back to my starter simple, not modular nix config, just a few files, home-manager, and flakes.
>
> In those few files I have everything system-related that makes sense to do via the nix way, and I use home-manager to configure other stuff that makes sense to do the nix way.
>
> For other stuff, like nvim, hyprland, waybar, dunst, and other relatively big configs I use the native format and just create symlinks via home-manager.
>
> From my experience I found this way to be the simplest one, the most pleasant to deal with, thus the best one.

A response from community member [Adk]:

> That's exactly how I do it, and I also find it to be the best way to handle things. There is no point in falling for the meme of "everything must be nix"

And from [Choko](https://github.com/chocoblocko9):

> I don't think you should confuse "writing it in nix" and "doing it the nix way".
> Who said symlinking a file isn't the nix way?
> home-manager isn't part of nixos you know.
> By and large nixos doesn't deal with home management at all.
> Basically I'm trying to say symlinking stuff is perfectly fine.
> I use it for some stuff, don't use it for others.
> You've already figured out where it's useful and where it's not.

And it turned out that after discussing it with multiple people in the community, most of them had arrived at the same conclusion through their own experience. It is not just me.

Also, I had been confusing "doing everything the Nix way" with "writing everything in Nix." This is a very common mistake. The urge to write every single config in the Nix language is a natural phase you go through, and most people end up stepping back from it once they run into the complexity, messiness, and maintenance headaches that come with pasting native config formats as raw strings inside `.nix` files.

---

## The Approach That Actually Works

After all of that, I ended up back at something simple.

My current single-machine setup looks like this:

```
~/nixos
├── configuration.nix
├── flake.lock
├── flake.nix
├── hardware-configuration.nix
├── home.nix        (points symlinks -> ~/dotfiles)
└── init.sh

~/dotfiles
├── btop
├── dunst
├── fastfetch
├── hypr
├── foot
├── nvim
├── rmpc
├── rofi
├── scripts
├── tmux
├── waybar
├── yazi
└── zathura
```

Everything system-level that Nix handles well, like packages, services, and system configuration, lives in the Nix config. Everything application-level with a substantial native config format, like Neovim, Hyprland, and Waybar, lives in `~/dotfiles` and home-manager creates symlinks to those files. Clean, simple, and easy to understand at a glance.

The key insight is that NixOS does not require you to configure everything through Nix. It requires you to define your system through Nix. Those are different things, and conflating them leads to exactly the kind of overengineering I fell into. There is no "right" way in NixOS. The right way is the one that makes sense for your actual situation.

---

## A Different World

Here is what has genuinely changed after a month.

The interaction model is different, and I have stopped trying to resist it. On Arch, I interacted with my system constantly: editing files directly, running commands, making incremental changes. It was hands-on in a way that felt satisfying. On NixOS, most of that activity has been replaced by editing a small set of configuration files. At first this felt like losing something. Now it feels like the point.

The declarative model is simpler, once you learn the language to express what you want. You say what you want once, and it works. You do not debug the same issue twice. You do not lose configurations because you forgot to document them. You do not have to remember anything, because everything is already written down by definition. And the consistency of it, everything in the same format, everything in one place, creates a sense of organization and control that I never had with Arch, despite spending years trying to achieve it.

I also found that when I want to understand something at a deeper level, nothing stops me. I can look at the generated configuration files. I can read the NixOS module source. The difference is that I am not forced to engage with those layers for every routine task. NixOS gives you the option. That turns out to be far better than Arch's implicit requirement that you engage with everything manually all the time.

And over time, you build the same kind of intuition on NixOS that you built on classic distros. It just looks different: you start to know which options exist and what they are likely to be called, you develop a sense for where to look when something breaks, and you build mental models of how parts of your configuration interact. The knowledge accumulates. It is just Nix knowledge instead of FHS knowledge, and that is a perfectly fine trade.

---

## What I Miss, What I Don't

Honestly, I miss almost nothing.

I do not miss losing configurations. I do not miss spending 30 minutes restoring my system after a reinstall only to discover things I forgot to document. I do not miss the anxiety of running a big update after weeks of skipping them, wondering what will break. I do not miss fixing the same issue twice because I forgot I had already fixed it once.

What I miss, a little, is the feeling of raw interaction with the system: going into `/etc`, editing a file, seeing the change take effect immediately. There is something tactile about that workflow that NixOS does not replicate. But the more I use NixOS, the more I recognize that feeling for what it was: familiarity, not superiority. I was comfortable with the imperative model because I had spent years with it, not because it was actually better.

On NixOS, I am building a new familiarity. And it is already starting to feel like home.

---

## Is It Worth Committing To?

Yes. Unambiguously.

It is hard at first, and I still feel like I know very little after almost two months of using NixOS. The learning curve is real. You will hit footguns. You will spend hours on things that should take five minutes. You will question your choices, probably more than once. I had a flash drive with an Arch ISO ready to go, and I came very close to using it. After that moment, I understood why some people do not like NixOS and switch back to Arch or whatever FHS-compatible distro they prefer.

But the core idea is correct, and once you internalize it, the tool starts to feel genuinely powerful. The problems you encounter are real problems, but they are solvable, and solving them makes you better at using the tool rather than just patching the same recurring issues. The community is excellent, and going to them when you are stuck or uncertain is one of the best things you can do.

I also cannot stop talking about NixOS at this point. I find myself explaining it to other Linux users, making the case for declarative configuration, and being mildly baffled that the imperative model is still the default. I do not fully understand why it dominates, given how much better the alternative feels once you have lived with it. That is probably a whole other post.

For now: if you are on the fence, stay. Ask the community. Struggling as a beginner is just part of becoming proficient, and the other side of that curve is worth the effort.

---

Here are the resources I found most helpful:

**Learning**
- [Vimjoyer's YouTube channel](https://www.youtube.com/@vimjoyer)
- [NixOS Discord server](https://discord.gg/pJXzBqjzBx) (unofficial, but the largest one)
- [Nix Pills](https://nixos.org/guides/nix-pills/)
- [Nix by Example](https://ops.functionalalgebra.com/nix-by-example)
- [Zero to Nix](https://zero-to-nix.com) (a guide to learning Nix and flakes)
- Other YouTube videos and blog posts
- LLMs work surprisingly well as a search engine for Nix too

**Day-to-day essentials**
- [Official docs](https://nixos.org/manual/nixos/stable/)
- [Package search](https://search.nixos.org)
- [home-manager options search](https://home-manager-options.extranix.com/)
- [nixos.org](https://nixos.org)

**Example configs to learn from**
- [nix-starter-configs](https://github.com/Misterio77/nix-starter-configs)
- [More examples on the NixOS wiki](https://nixos.wiki/wiki/Configuration_Collection)
