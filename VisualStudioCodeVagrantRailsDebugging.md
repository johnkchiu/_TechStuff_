# How to Debug Rails on Vagrant Guest from Visual Studio Code on Host

## Overview

## Steps

1. Install Ruby for Visual Studio Code - https://marketplace.visualstudio.com/items?itemName=rebornix.Ruby
1. Modify `Vagrantfile` to forward port

        config.vm.network "forwarded_port", guest: 1234, host: 1234 , auto_correct: true
1. Restart Vagrant guest after the configuration change.
1. Add debugging gems to `Gemfile`

        gem 'ruby-debug-ide', group: [:development,:test]
        gem 'debase', group: [:development,:test]
1. Rerun `bundle install`
1. Launch rdebug-ide

        rdebug-ide --host 0.0.0.0 --port 1234 --dispatcher-port 26162 -- bin/rails server -b 0.0.0.0
1. In Visual Studio Code, open the `Debug` panel.  Setup your `launch.json` and with the correct `remoteWorkspaceRoot` with path of Vagrant guest.

        ...
                {
                    "name": "Listen for rdebug-ide",
                    "type": "Ruby",
                    "request": "attach",
                    "cwd": "${workspaceRoot}",
                    "remoteHost": "127.0.0.1",
                    "remotePort": "1234",
                    "remoteWorkspaceRoot": "/home/vagrant/root"
                },
        ...
        
