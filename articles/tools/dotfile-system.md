Title: My dotfile system
Tags: dotfiles, tools, script, linux, unix
Date: 2016-04-01
Summary: I hate to be machine dependent. For me a computer is just a tool to get things done. If one breaks down for various reasons or I switch machine I want to have my tools and setup without any manual configuration.
Series: Dotfiles
Repo: https://github.com/tiborsimon/dotfiles
Release_link: https://github.com/tiborsimon/dotfiles/releases/latest

Have you ever felt crippled on a new machine you've just logged in for the first time? Tools missing, and if not, they have weird default configurations. Before you can start to work, you need to move in that machine: download all your needed tools, configure them one by one by hand. You quickly realize this is not the best way you can do..

<div class="divider">Solution one</div>

# Externally saved configuration

You can speed up the initial configuration if you have a previously saved config file set in a machine independent storage, say Dropbox or a USB drive.

These files are generally called __dotfiles__, as command line tools often expect their configuration files to start with a dot indicating the file is hidden for normal use. Dotfiles are generally located in the current user's _home_ directory. When you start using a new machine, you simply  copy your saved config files to your home directory and you are home. 

Besides the custom configuration files, you can also save out a list of installable programs and tools that you want to have by default on a machine. Some package manager tools supporst installing from files, but if not, you can write a small script that will install your tools one by one. You can create a `packages.txt` file for this purpose:

<div class="code-name">packages.txt</div>
```
git
mc
vim
```

The one-by-one installer script could be something like this:

```
for app in $(cat path/to/your/packages.txt); do 
  echo "Installing $app.."
  if $YOUR_PACKAGE_MANAGER install -y $app>temp_log_file 2>&1; then
    echo "$app installed."
  else
    echo "$app installation failed! Error: \n$(cat temp_log_file)"
  fi
  rm -f temp_log_file
done
``` 

You might need to have administrator priviliges to install packages on your machine. You have to replace the path to your _packages.txt_ file and your system's package manager.

### Conclusion

This method is semi-manual, as you have to copy the files, and run the commands yourself. It would be much more elegant if you would have a _one-command deployment_ solution. An another big drawback is the lack of version control (unless you have a local git repo on you USB drive). It is always a good idea to use versioning __anything__ you do in you machine in case of an accident[^1].


<div class="divider">Solution two</div>

# Hosted and versioned configuration

The next improvement step is to put your config files in a version control system, and host it on the internet. If you want a public configuration, you can't find better than place than GitHub. However, if you want your config to be in a private place, I can suggest using BiBucket, as it privides free private repositories.

Eighter versioning provider you choose, you will end up having an easier to deployable config system. You can clone your setup on a new machine, and you will have your setup ready to use.

There is only one problem with this approach. If you clone your repo into your home folder, that will lead to a state, when your entire home folder became a repository, 

[^1]: Oh the irony.. Exactly after I typed this sentence my machine freezed :D Commit often, people! 
