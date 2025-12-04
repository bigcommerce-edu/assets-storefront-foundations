# Lab - Getting Started with the Stencil CLI

**Prerequisites**

To successfully complete this tutorial, you will need the following:

* A BigCommerce store admin user with the "Create store-level API accounts" permission
* Node.js 20 or later ([nvm](https://github.com/nvm-sh/nvm) recommended)
* The [Git](https://git-scm.com/) CLI
* A command-line interface on your local machine

## Step 1: Create an API Account

1. In the BigCommerce control panel, **navigate** to Settings &gt; API &gt; Store-level API accounts.
2. **Click "** Create API account" and **select**"Stencil CLI token"as the token type.
3. **Enter** a name for the account.
4. **Select**"publish theme" as the Stencil CLI access level.

You'll use this API account in a later lab to bundle and publish your theme. For proper access control, you should create a separate account with the "local development only" access level for any team members who should have permission to develop on the theme but not upload it.

5. **Save** the account and **record** the access token that is displayed in the resulting modal.

## Step 2: Install Stencil CLI

This step only needs to be performed once for a specific version of Node.js.

1. **Run** the following command to install the Stencil CLI tool.

```bash
npm install -g @bigcommerce/stencil-cli
```

Remember to make sure you have Node.js 20 or later running before installing the package. If you're using Node Version Manager:

`nvm use 20`

The command will install the _stencil-cli_ package globally. This will allow you to use it in multiple Stencil theme projects using the same version of Node.js.

**ARM Based Macs**

For ARM based Macs, you will need to run the following before installing the package:

`arch -x86_ 64 /bin/zsh`

**Installing Dependencies on Windows**

See the [BigCommerce documentation](https://developer.bigcommerce.com/docs/storefront/stencil/cli/install#installing-on-windows) for more details on options for installing dependencies on Windows before installing the Stencil CLI package.

2. **Test** the _stencil_ command and **verify** that a list of possible sub-commands is output.

```bash copy
stencil -h
```

## Step 3: Clone the Cornerstone Theme

1. **Run** the following command to clone the Cornerstone default theme from [GitHub](https://github.com/bigcommerce/cornerstone/) to a local location.

```bash copy
git clone https://github.com/bigcommerce/cornerstone.git
```

2. **Install**_npm_dependencies.

```bash copy
cd /path/to/theme
npm install
```

You should now have a complete copy of the Cornerstone theme source files, with dependencies installed in the _node_ modules_directory.

## Step 4: Initialize Stencil

This step only needs to be performed once for a given Stencil theme project locally.

You'll need the access token you recorded after creating the API account in your control panel.

1. In your theme working directory, **run** the following command.

```bash copy
stencil init --url <store_url> --token <access_token> --port 3000 --packageManager npm
```

This _init_ command will generate a configuration file connecting your local theme package with your store.

* Replace **&lt;store_url&gt;** with the fully qualified URL of your Stencil storefront (for example, "https://&lt;store-name&gt;.mybigcommerce.com").
* Replace **&lt;access_token&gt;** with your recorded access token.
* The **--port** option specifies the local port the server will run on (for example, _localhost:3000_).
* The **--packageManager** option specifies which package manager to use for dependencies.

You may also choose to run the command with no options:

`stencil init`

This will result in interactive prompts allowing you to provide appropriate values.

2. **Verify** that the file _config.stencil.json_ has been generated in your main theme directory.

## Step 5: Start the Live Preview

1. In your theme working directory, **run** the following command to start a local dev server with a live preview of your theme.

```bash copy
stencil start
```

2. If necessary, **choose** a storefront to run from the interactive prompt.

After the dev server starts, you should see output printing the local URL(s) you can use to access the preview.

3. **Browse** to the main "Local" access URL printed in the output. This should be _http://localhost:3000_ if you used the 3000 port in the _init_ command. **Verify** that your storefront is loaded in the browser.

You should leave the dev server process running in your terminal as you customize and preview your theme. When you're ready to stop the server, simply cancel the process (Ctrl+C).

Run `stencil start` from within your theme directory at any time to restart the dev server.
