Local Development Environment – WSL2
=========================================

.. contents:: Contents:
   :local:

This page covers how to set up Windows with WSL2 for development of this project.

For most of the steps, all you need to do is clicking the copy button at the top-right corner of a snippet box, pasting
to the WSL2 terminal and hit :kbd:`Enter`. Occasionally, you need to replace the placeholders in the command with their
actual values. The placeholders are all of the ``${***}`` format, i.e. you can export environment variables of
the same names and let shell do the substitution for you.

Enable WSL2
----------------------------------------

Follow the `Microsoft official doc`_ to enable WSL2 on a Windows machine. Make sure that you enabled WSL *version 2*
and installed a Ubuntu distro versioned 23.04 or above. 

General developer tools
-------------------------

Perform the initial :command:`apt update` and :command:`apt upgrade` and install some baseline tools.

.. code-block:: bash

   $ sudo apt update && sudo apt upgrade -y
   $ sudo apt install zip unzip curl -y

Install the :program:`zsh` shell.

.. code-block:: bash

   $ sudo apt install zsh -y

Switch to :program:`zsh` as the default login shell.

.. code-block:: bash

   $ chsh -s $(which zsh)

**Now exit the current WSL2 terminal session and open it again.** Otherwise the default login shell won't change.

Install :program:`git`.

.. code-block:: bash

   $ sudo apt install git -y

Set up :program:`git` global username and email.

.. code-block:: bash

   $ git config --global user.name ${YOUR_USER_NAME}
   $ git config --global user.name ${YOUR_EMAIL}

Install GitHub CLI.

.. code-block:: bash

   $ type -p curl >/dev/null || (sudo apt update && sudo apt install curl -y)
   $ curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
   $ && sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
   $ && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
   $ && sudo apt update \
   $ && sudo apt install gh -y

Install :program:`zsh-autosuggestions`.

.. code-block:: bash

   $ git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions

Python and Node tools
-----------------------

Install :program:`pyenv`.

.. code-block:: bash

   $ sudo apt install build-essential libssl-dev zlib1g-dev \
   $ libbz2-dev libreadline-dev libsqlite3-dev curl llvm \
   $ libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev \
   $ libffi-dev liblzma-dev -y
   $ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
   $ pushd ~/.pyenv && src/configure && make -C src && popd

Install :program:`pipx`.

.. code-block:: bash

   $ sudo apt install pipx -y
   $ pipx ensurepath

Install :program:`poetry`.

.. code-block:: bash

   $ pipx install poetry

Install :program:`nvm`.

.. code-block:: bash

   $ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash

Get baseline dot files. **Back up your own** :file:`~/.zshrc` **and** :file:`~/.zprofile`
**files before executing the following commands.**

.. code-block:: bash

   $ curl -s -o ~/dotFiles.zip https://raw.githubusercontent.com/kxue43/zsh-dot-files/master/dotFiles-no-rbenv.zip
   $ pushd ~ && unzip -o dotFiles.zip && rm dotFiles.zip && popd

**Now exit the current WSL2 terminal session and start a new one before proceeding.**
This allows the new dot files to take effect.

Set up this project
-------------------------

Install the Python and Node versions needed for this project.

.. code-block:: bash

   $ pyenv install 3.12.4
   $ nvm install 20.11.1 --default

Login to GitHub via its CLI. Choose the HTTPS authentication method and follow prompts.

.. code-block:: bash

   $ gh auth login

Clone the project repo **into WSL2**.

.. code-block:: bash

   $ git clone https://github.com/kxue43/fs-walk

Execute the following **under the project root directory**.

.. code-block:: bash

   $ pyenv local 3.12.4
   $ poetry env use $(pyenv which python)
   $ poetry config --local virtualenvs.in-project false
   $ poetry install
   $ pre-commit install -t pre-commit -t post-merge

VSCode extensions
-------------------

WSL2 is basically a Linux VM running inside a Windows operating system. The WSL2 VM doesn't have a GUI, but VSCode
can open a project folder in WSL2, thus providing enough of a graphical interface for developers. All it takes
is installing the ``ms-vscode-remote.remote-wsl`` VSCode extension and opening WSL2 project folders in a specific
way. Refer to `WSL 2 with Visual Studio Code`_ for more details.
**This is the best setup for Windows users to work on this project.**

There are GitHub Actions workflows that perform static checking and testing for this project.
Unless all checks pass, no PR is allowed to merge. To know whether there are errors
*during development* and to get an overall better developing experience, it is recommended to use VSCode and
have the following extensions installed. The extensions below integrate seamlessly with the static checkers of
the project. **Windows users should install these in the "WSL2 VSCode server".**

- ``ms-python.black-formatter``
- ``tamasfe.even-better-toml``
- ``ms-python.flake8``
- ``ms-toolsai.jupyter``
- ``ms-toolsai.vscode-jupyter-cell-tags``
- ``ms-toolsai.jupyter-keymap``
- ``ms-toolsai.jupyter-renderers``
- ``esbenp.prettier-vscode``
- ``ms-python.vscode-pylance``
- ``ms-python.python``
- ``ms-python.debugpy``
- ``KevinRose.vsc-python-indent``

.. _Microsoft official doc: https://learn.microsoft.com/en-us/windows/wsl/install
.. _WSL 2 with Visual Studio Code: https://code.visualstudio.com/blogs/2019/09/03/wsl2
