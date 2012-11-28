Based on Zach Holman's excellent example here's my attempt to make my development machine configuration reusable.

Setting up a new development system for my use at Carbon Five still requires a fair number of steps which might be worth automating.

- `mkdir ~/Projects` I structure my git repositories as `~/Projects/<client>/<repo>`
- turn on FileVault
- setup TimeMachine
  - set ignored directories (DropBox, Projects, anything else already backed up outside TimeMachine)
- install XCode
- install XCode command line tools
- install [brew](http://mxcl.github.com/homebrew/)
- install [sublime](www.sublimetext.com/2)
  - install [sublime package manager](http://wbond.net/sublime_packages/package_control)
- install DropBox
- install Chrome
- install Firefox
- install a password manager
- install chat clients
- install [mou](http://mouapp.com/)
- install a GUI git client
- generate and add a public key to github

    ```sh
    ssh-keygen -t rsa -C "your_email@youremail.com"
    pbcopy < ~/.ssh/id_rsa.pub
    ```
  
- Set zsh as my default shell:

    `chsh -s /bin/zsh`

- dotfiles

  - install

      ```sh
      git clone https://github.com/jonah-carbonfive/dotfiles.git ~/.dotfiles
      cd ~/.dotfiles
      ```

  - update git username

      ```sh
      open git/gitconfig.symlink
      ```

  - apply configuration
  
      ```sh
      script/bootstrap
      osx/set-defaults.sh
      sublime2/setup
      ```

- install a current ruby

    ```sh
    rbenv install -l
    rbenv install 1.9.3-p327
    rbenv global 1.9.3-p327
    ```

- install postgres

    ```sh
    brew install postgres
    initdb /usr/local/var/postgres -E utf8
    mkdir -p ~/Library/LaunchAgents
    cp /usr/local/Cellar/postgresql/9.2.1/homebrew.mxcl.postgresql.plist ~/Library/LaunchAgents/
    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
    ```

- install redis

    ```sh
    brew install redis
    ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents
    launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
    ```

- install useful gems in global ruby

    ```sh
    gem install bundler
    gem install lunchy
    rbenv rehash
    ```

- update `~/.dotfiles/README.markdown` with anything missing from the list above
- get some work done

===================================

# holman does dotfiles

## dotfiles

Your dotfiles are how you personalize your system. These are mine. The very
prejudiced mix: OS X, zsh, Ruby, Rails, git, homebrew, rbenv, vim. If you
match up along most of those lines, you may dig my dotfiles.

I was a little tired of having long alias files and everything strewn about
(which is extremely common on other dotfiles projects, too). That led to this
project being much more topic-centric. I realized I could split a lot of things
up into the main areas I used (Ruby, git, system libraries, and so on), so I
structured the project accordingly.

If you're interested in the philosophy behind why projects like these are
awesome, you might want to [read my post on the
subject](http://zachholman.com/2010/08/dotfiles-are-meant-to-be-forked/).

## install

Run this:

```sh
git clone https://github.com/holman/dotfiles.git ~/.dotfiles
cd ~/.dotfiles
script/bootstrap
```

This will symlink the appropriate files in `.dotfiles` to your home directory.
Everything is configured and tweaked within `~/.dotfiles`, though.

The main file you'll want to change right off the bat is `zsh/zshrc.symlink`,
which sets up a few paths that'll be different on your particular machine.

You'll also want to change `git/gitconfig.symlink`, which will set you up as
committing as Zach Holman. You probably don't want that.

## topical

Everything's built around topic areas. If you're adding a new area to your
forked dotfiles — say, "Java" — you can simply add a `java` directory and put
files in there. Anything with an extension of `.zsh` will get automatically
included into your shell. Anything with an extension of `.symlink` will get
symlinked without extension into `$HOME` when you run `rake install`.

## what's inside

A lot of stuff. Seriously, a lot of stuff. Check them out in the file browser
above and see what components may mesh up with you. Fork it, remove what you
don't use, and build on what you do use.

## components

There's a few special files in the hierarchy.

- **bin/**: Anything in `bin/` will get added to your `$PATH` and be made
  available everywhere.
- **topic/\*.zsh**: Any files ending in `.zsh` get loaded into your
  environment.
- **topic/\*.symlink**: Any files ending in `*.symlink` get symlinked into
  your `$HOME`. This is so you can keep all of those versioned in your dotfiles
  but still keep those autoloaded files in your home directory. These get
  symlinked in when you run `rake install`.
- **topic/\*.completion.sh**: Any files ending in `completion.sh` get loaded
  last so that they get loaded after we set up zsh autocomplete functions.

## add-ons

There are a few things I use to make my life awesome. They're not a required
dependency, but if you install them they'll make your life a bit more like a
bubble bath.

- If you want some more colors for things like `ls`, install grc: `brew install
  grc`.
- If you install the excellent [rbenv](https://github.com/sstephenson/rbenv) to
  manage multiple rubies, your current branch will show up in the prompt. Bonus.

## bugs

I want this to work for everyone; that means when you clone it down it should
work for you even though you may not have `rbenv` installed, for example. That
said, I do use this as *my* dotfiles, so there's a good chance I may break
something if I forget to make a check for a dependency.

If you're brand-new to the project and run into any blockers, please
[open an issue](https://github.com/holman/dotfiles/issues) on this repository
and I'd love to get it fixed for you!

## thanks

I forked [Ryan Bates](http://github.com/ryanb)' excellent
[dotfiles](http://github.com/ryanb/dotfiles) for a couple years before the
weight of my changes and tweaks inspired me to finally roll my own. But Ryan's
dotfiles were an easy way to get into bash customization, and then to jump ship
to zsh a bit later. A decent amount of the code in these dotfiles stem or are
inspired from Ryan's original project.
