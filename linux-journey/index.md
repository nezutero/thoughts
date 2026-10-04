# My Linux Odyssey: How I Ended Up on NixOS

*Last updated: 21 July 2026*

*A personal account of distro-hopping, window managers, and eventually finding NixOS.*

---

## Part 1: How It All Started

I started using Linux about four years ago. My very first distribution was Fedora, followed by Ubuntu, which I used for about a month before switching back. The main reason I left Ubuntu was its interface; I simply didn't like it. But beneath that was a deeper frustration: bloat. Ubuntu came pre-installed with so much software that, as someone still finding their footing in Linux, I felt completely lost. I tried removing things, but the sheer volume of it all was overwhelming.

That led me back to Fedora. I had heard it was a more sophisticated distribution, and that reputation drew me in. I ended up spending several months there, and during that time I fell deep into the rabbit hole of GNOME customization. Using GNOME Tweaks and extensions from the GNOME extension store, I spent hours adjusting every small detail I could find: dozens of tweaks, custom layouts, themes, you name it. I still have screenshots from that period.

{{< gallery src="./images/1.webp" name="Fedora GNOME Tweaks 1" >}}
{{< gallery src="./images/2.webp" name="Fedora GNOME Tweaks 2" >}}
{{< gallery src="./images/3.webp" name="Fedora GNOME Tweaks 3">}}

But the same core frustration eventually came back. I still felt like I didn't have real control over my system. I wanted to decide exactly what went on it, from the display protocol to the window manager to individual applications. And that desire led me to Arch Linux.

---

## Part 2: Entering Arch Linux

The appeal of Arch Linux is something many Linux users will recognize immediately: a minimal base, total control, and the expectation that you actually know what you're doing. I started using it at the beginning of September 2023, and from the start it felt like the right fit.

The first thing I wanted to try was something I had been experimenting with near the end of my time on Fedora: switching from a full desktop environment to a standalone window manager. The timing felt perfect. A new operating system and a completely new way of interacting with my desktop, all at once. I set up bspwm on X11, with Polybar as my status bar, Zsh as my shell, and a solid collection of plugins including syntax highlighting and autocompletion. The whole setup worked exactly the way I wanted it to.

{{< gallery src="./images/4.webp" name="Arch TTY memories">}}

---

## Part 3: Going Wayland -- Hyprland and Finding My Tools

After about four months on X11, I decided to make the switch to Wayland. At the time, even though it was only about three years ago, it was still less common than it is today; it felt more like an enthusiast choice than a mainstream one, and a less stable one at that. I started using Hyprland as my compositor, paired with Waybar, and I have been refining that configuration ever since, gradually improving it over the years.

{{< gallery src="./images/5.webp" name="Voice acting home studio on arch">}}

During that time I also did a lot of experimenting with other tooling. On the terminal side, I went through Alacritty, Kitty, Foot (a minimal terminal written in C), and Ghostty (written in Zig, btw). For shells, I gave Fish a serious try before eventually returning to Zsh. After all of that, I settled on Kitty as my primary terminal. The deciding factor was image rendering: Kitty supports displaying images and video previews inline, which matters a lot to me because I use the terminal for almost everything, from file browsing and listening to music to watching videos. Alacritty is still the terminal I admire most on a philosophical level, given its focus on minimalism and performance, but the missing image support is a dealbreaker for my specific workflow.

**[21 July 2026] Update:** A few weeks ago, I've switched to [foot](https://github.com/dnkl/foot), which is faster, uses less memory, wayland native, and is quite minimalist -- unlike kitty. It supports image previews and is written in C. It's really great.

{{< gallery src="./images/6.webp" name="Latest arch rice">}}

---

## Part 4: Why I Left Arch Linux

After almost three years, two things pushed me away from Arch.

### Crashes and Fragility

Similarly to "with great power comes great responsibility," with great freedom comes great responsibility too, and the burden of managing everything yourself. Arch is amazing: rolling releases, the latest packages, a large repository, and the AUR (Arch User Repository), which was the largest package collection available before the Nix package set. Even though this is something close to a miracle for users who want real control over their systems, it also has its cost. At the end of the day, it is all about tradeoffs; the combination of pros and cons is what makes something genuinely great.

I experienced two major system failures during my time with Arch. I am the kind of user who constantly tinkers: trying new packages, removing old ones, cleaning the cache, experimenting with obscure forks and unofficial software. Most of the time this was fine. But I also had stretches where I skipped updates for a month or two, kept making changes in the meantime, and then ran a large update all at once. The last time this happened, something went wrong with the bootloader and the system simply refused to boot.

With no graphical interface available, I was stuck in a TTY with no browser, manually typing error messages into my phone to search for solutions online. I eventually sorted it out, and I will say honestly: it was a genuinely valuable experience. But it was also frustrating enough that I had no desire to go through it again. The first failure, which happened earlier in my Arch days, was similar in spirit, though the exact cause is hazier in my memory.

### System Rot and the Imperative Model

The deeper issue, though, was not the crashes themselves. It was something more gradual.

Over time, I noticed my Arch system no longer resembled what I had originally installed. Years of updates, deletions, installations, and tinkering had left it in a state I couldn't fully account for. Even with regular cache cleaning and orphan package removal, the system had quietly drifted away from any coherent starting point. I think of this as system rot: arguably just the natural lifecycle of any operating system, but something I found genuinely difficult to accept. The machine I was running after two or three years on Arch was quite different from the one I had set up, and not entirely in ways I could explain or retrace.

Part of the problem is how traditional Linux distributions handle configuration. The model is fundamentally imperative: you run commands, set flags, edit files, and hope you remember everything you did. Some things can be managed declaratively through config files, and then you collect those in a dotfiles repository and manage them with symlinks or tools like GNU Stow, chezmoi, or (god help you) Ansible YAML files. But plenty of things cannot be handled that way, which means you end up with shell scripts, install scripts, and a patchwork of automation that needs constant upkeep.

My own install script worked cleanly about half the time after a reinstall. Even on a minimal system, getting back to my previous state took around 30 minutes and always involved surprises. Some things I had intentionally left out of the script because they felt too situational to automate properly. Others I had simply forgotten to add. And some changes I had made over time were just gone, because I had never written them down anywhere. The system had become, in a word, chaotic, and there was no clean way to recover a true picture of what it was supposed to look like.

---

## Part 5: Discovering NixOS

I had known about NixOS for a while before I seriously looked into it. A friend of mine first mentioned it about three years ago, right around when I was getting started with Arch. We used to talk about Linux a lot on Discord, and he would share his experience using NixOS alongside whatever I was tinkering with at the time. The concept stuck with me, though my understanding of it back then was hazy and incomplete. I knew you could define your whole system in a few files and more or less never have to think about it again, but I didn't really grasp what that meant in practice.

Over the following years, NixOS kept coming up in articles and conversations, resurfacing whenever I least expected it. Eventually, I wanted to try something new and more advanced; I was also increasingly frustrated with the imperative model and the anxiety of potentially losing my system configuration all over again.

### Side Story: Self-Hosting and the Limits of Imperativeness

As a concrete example, at some point I decided to explore self-hosting. I rented a server at Hetzner, and at the time their options were Ubuntu, Debian, Fedora, and if I remember correctly, OpenSUSE. You can of course install whatever you like on a VPS if you are willing to put in the work, but at that point I was not a NixOS user and did not think it was worth the hassle. So I just used Debian.

I set up a reverse proxy and SSL certificates with Caddy, then configured all my self-hosted services: Immich, Uptime Kuma for monitoring, Nextcloud, Gitea, and Vaultwarden. Setting up a custom domain was straightforward enough. All of it ran in Docker, with the requisite pile of YAML files. After a week of managing it, though, I decided to stop. It was an interesting experience, but I concluded that my infrastructure could be much simpler; storing things locally and using Proton Drive was enough for me.

A few months later I decided to try self-hosting again, but more deliberately. It turned out I had none of my old configs, simply because I had not backed them up; I thought I was done with self-hosting for good, so I hadn't bothered. But I wanted to build something better than my first attempt, so I started from scratch and redid everything. Eventually, after a few days, the fun part was over and I was left wondering why I needed any of it, because practically speaking I didn't. I just wanted to tinker and build something, not actually manage and rely on it. So that time I backed up my configs before shutting everything down, knowing I would probably want to do it all again someday.

Months later, full of drive and determination to ditch big tech entirely and host my own services, I was confident I could have everything running in about 30 minutes. I had my configs backed up this time. I was wrong. There were issues I had never encountered before and therefore had never documented: firewall conflicts, Docker setup quirks, ports that needed opening again. I spent hours fixing things I thought were already solved. After that, I had had enough of the imperative model, and somewhere in that frustration the thought hit me: what if I used NixOS? This is exactly the kind of problem it is designed to eliminate.

---

### Back to the Main Story

I was wrestling with my install script, growing increasingly frustrated: things I had forgotten to add, steps that behaved differently on a fresh install, manual interventions I had to remember every single time. While digging around for a better approach, I stumbled upon an article that finally made things click. I spent a few hours reading about NixOS properly, and what I found was exactly what I had been looking for without quite knowing it.

The core appeal of NixOS is its declarative, purely functional approach to system configuration. Your entire system, including packages, services, user settings, and dotfiles, is defined in configuration files. The practical results of this are significant:

- **Reproducibility**: you can rebuild your exact system on any machine in minutes.
- **Atomicity**: updates either fully succeed or fully roll back. No more half-broken states.
- **Accountability**: every change is explicit and traceable. The system does not drift.

For someone who had spent years fighting system rot, this felt like a genuine revelation. Everything is defined; you know exactly what is installed and why. No more dependency hell.

One thing worth flagging upfront is that switching to NixOS means a lot of your existing Linux knowledge does not directly apply. The interaction model is genuinely different from anything in the traditional Linux world; the system adds an abstraction layer over the way you configure and use it, and the nature of the problems you encounter changes completely. That can be disorienting at first, and it is something to be prepared for rather than caught off guard by.

The Nix language itself also has a steep learning curve, which was the main thing giving me pause before I made the switch. The documentation, whilst it exists, can be inconsistent and hard to navigate. One aspect I find both appealing and occasionally maddening is that Nix often offers multiple ways to solve the same problem. On one hand, this means almost nothing is truly unsolvable on NixOS; there is always some approach that works, and you will rarely hit a wall with no door in it. On the other hand, it means you can spend hours searching for the right solution when a simpler one was sitting elsewhere the whole time. This is part of mastering the tool, though, and it is no different from the learning curve I went through with Arch Linux years ago.

I have now been using NixOS for about three weeks. My expectations going in were perhaps a little too idealistic. I had half-convinced myself it would be a silver bullet, and of course no tool really is. But the core idea is genuinely powerful, and I believe that once you have properly invested in learning it, it gets remarkably close to one.

{{< gallery src="./images/7.webp" name="My current NixOS setup">}}

I'm not fully committed to the pure Nix way of configuring everything; I configure the majority of my system through Nix, but I keep my dotfiles and application-specific configurations separate. Things like Hyprland, rofi, and Neovim live in their own dotfiles directory rather than being fully declared in Nix. I use home-manager to manage these, and it simply creates symlinks from my dotfiles repository into the appropriate locations in my home directory.

This approach lets me draw a clear line between system-level configuration and per-application settings. Dotfiles stay flexible and portable, they're version-controlled separately and can easily be shared or adapted across different systems without being tightly coupled to my NixOS declarations. Meanwhile, Nix handles the heavier lifting: managing packages, setting up system services, and declaring environment variables. It's a pragmatic middle ground that feels simpler and cleaner to me than trying to Nix-ify every single configuration file.

Do I think I might eventually go all-in and become a full NixOS purist? Maybe. But for now, this hybrid approach works well. It keeps my dotfiles readable and maintainable while still benefiting from Nix's declarative strengths where it matters most. There's no single "right way" to manage NixOS and this is just one of many valid approaches, and it suits my workflow.

- My dotfiles: https://github.com/nezutero/dotfiles

The next post will be the second part of this one, focusing on my experience with NixOS. It will cover my first month of daily use: what worked, what surprised me, where the rough edges are, what I miss and why, and what I don't miss. I will try to answer the question: is it worth committing yourself to learning NixOS and the Nix language? I had this question before switching, and I've continued thinking about it because NixOS is hard and complex -- these questions arise naturally. This will be the topic of the [next post](link).

Thank you for reading.

---

Read the [second part](https://nezutero.dev/almost-went-back-one-month-on-nixos)
