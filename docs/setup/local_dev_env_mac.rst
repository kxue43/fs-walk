Local Development Environment – MacBook
=========================================

.. contents:: Contents:
   :local:

This page covers how to set up a MacBook for development of this project. For most of the steps, all you need to do
is clicking the copy button at the top-right corner of a snippet box, pasting to the MacBook terminal
and hit :kbd:`Enter`. Occasionally, you need to replace the placeholders in the command with their actual values.
The placeholders are all of the ``${***}`` format, i.e. you can export environment variables of the same names
and let shell do the substitution for you.

General developer tools
-------------------------

Install :program:`homebrew` and perform initial upgrade and upgrade.

.. code-block:: bash

   $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   $ if [[ $(uname -m) == 'arm64' ]]; then
   $   eval $(/opt/homebrew/bin/brew shellenv)
   $ fi
   $ brew update
   $ brew upgrade

Install :program:`git`.

.. code-block:: bash

   $ brew install git

Set up :program:`git` global username and email.

.. code-block:: bash

   $ git config --global user.name ${YOUR_USER_NAME}
   $ git config --global user.name ${YOUR_EMAIL}

Install GitHub CLI.

.. code-block:: bash

   $ brew install gh

Install :program:`zsh-autosuggestions`.

.. code-block:: bash

   $ git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions

Python and Node tools
-----------------------

Install :program:`pyenv`.

.. code-block:: bash

   $ xcode-select --install
   $ brew install openssl readline sqlite3 xz zlib tcl-tk
   $ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
   $ pushd ~/.pyenv && src/configure && make -C src && popd

Install :program:`pipx`.

.. code-block:: bash

   $ brew install pipx
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

   $ curl -s -o ~/dotFiles.zip https://raw.githubusercontent.com/kxue43/zsh-dot-files/macbook/dotFiles-no-rbenv.zip
   $ pushd ~ && unzip -o dotFiles.zip && rm dotFiles.zip && popd

**Now exit the current terminal session and start a new one before proceeding.**
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

Clone the project repo.

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

There are GitHub Actions workflows that perform static checking and testing for this project.
Unless all checks pass, no PR is allowed to merge. To know whether there are errors
*during development* and to get an overall better developing experience, it is recommended to use VSCode and
have the following extensions installed. The extensions below integrate seamlessly with the static checkers of
the project.

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
