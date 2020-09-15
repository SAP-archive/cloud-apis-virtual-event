# Exercise 03 - Calling Workflow APIs

In this exercise you'll set up Workflow Management services in your trial account, and make a few API calls to the Workflow service instance. You'll do all this from the comfort of a dev space in the SAP Business Application Studio (App Studio), details of which were listed in the [prerequisites](../prerequisites.md).


## Steps

After completing the steps in this exercise you'll feel some familiarity with using the Workflow APIs in the Cloud Foundry (CF) environment of your SAP Cloud Platform trial account.

Because you're awesome and you've already set up your dev space according to the [prerequisites](../prerequisites.md), with the "MTA Tools" and "Workflow Management" SAP extensions installed, you're almost ready to start. First though, as we'll be interacting with the Workflow API from the command line within your App Studio dev space and learning about it that way, we'll install a couple of tools there to help us.


### 1. Add the `jq` tool to your App Studio dev space

We'll be dealing with JSON in this exercise, and [`jq`](https://stedolan.github.io/jq/) is a great way to parse it and extract values, on the command line and in shell scripts. We need to download the appropriate executable to our dev space environment, and we can do this by looking for the download link and using that in our dev space's terminal.

:point_right: Open up a new terminal with menu path **Terminal → New Terminal** or with keyboard shortcut ``Ctrl-` `` and create a directory in which to put this locally installed executable:

```shell
> mkdir -p $HOME/.local/bin/
```

> With all these shell prompt instructions, note that the real shell prompt that you are likely to see (such as `user: user $`) is not shown; instead, we just show `>` to keep things simple.

:point_right: Now download the executable, getting the link from the [download page](https://stedolan.github.io/jq/download/), specifically the one for the latest `jq` Linux 64-bit binary. Currently that is for `jq` version 1.6, and the URL is as shown, along with some typical output:

```shell
> curl -L curl -L https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 > $HOME/.local/bin/jq
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   632  100   632    0     0  15047      0 --:--:-- --:--:-- --:--:-- 15047
100 3861k  100 3861k    0     0  2690k      0  0:00:01  0:00:01 --:--:-- 3144k
>
```

:point_right: Make the downloaded `jq` file executable:

```
> chmod +x $HOME/.local/bin/jq
```

In order to be able to find this executable and run it, we need to add its location (the `$HOME/.local/bin/` directory) to our PATH; a convenient way to do this is to append a line to our `.bashrc` file, which is executed when we start a new Bash shell (which is when we open a new terminal in our dev space).

:point_right: Append a line to your `.bashrc` file like this (_make sure you use TWO greater-than signs, not just one, otherwise you'll overwrite rather than append to the file!_):

```
> echo 'export PATH=$HOME/.local/bin/:$PATH' >> $HOME/.bashrc
```

Now, opening up a new terminal, you'll be able to run jq:

```
> jq
jq - commandline JSON processor [version 1.6]

Usage:  jq [options] <jq filter> [file...]
        jq [options] --args <jq filter> [strings...]
        jq [options] --jsonargs <jq filter> [JSON_TEXTS...]
...
```

### 2. Add the `jwt-cli` tool to your App Studio dev space

We'll be creating OAuth 2.0 access tokens in the course of this exercise, which appear as long opaque strings of characters. But we can actually "parse" the tokens, with the help of a Java Web Token (JWT) command line tool.

The [`jwt-cli`](https://www.npmjs.com/package/jwt-cli) tool is available for Node.js and can be installed globally within your shell using the `--global` switch for the `npm` command.

:point_right: At your shell prompt in the terminal, install `jwt-cli` globally like this (some reduced output is shown here):

```
> npm install --global jwt-cli
/home/user/.node_modules_global/bin/jwt -> /home/user/.node_modules_global/lib/node_modules/jwt-cli/bin/jwt.js
[...]

+ jwt-cli@1.2.3
added 25 packages from 26 contributors in 2.563s
>
```

Because the dev space container already contains Node.js and `npm`, that's all you need to do.

:point_right: Check that you can execute the tool, by entering `jwt` at the prompt. You should see output similar to what's shown here:

```
> jwt
jwt-cli - JSON Web Token parser [version 1.2.3]

Usage: jwt <encoded token> --secret=<signing secret>

ℹ Documentation: https://www.npmjs.com/package/jwt-cli
⚠ Issue tracker: https://github.com/troyharvey/jwt-cli/issues
```


### 3. Clone and open this repository in your App Studio dev space

This repository (repo for short) contains some scripts that have been written for you, to help you get to know the Workflow APIs from the bottom up. So that you can run these scripts in the context of your App Studio dev space, it's now time to clone this repo there.

First, let's open the `projects/` directory as the workspace in your App Studio dev space.

:point_right: Use the **Open Workspace** button in the Explorer perspective and choose the `projects/` directory to open:

![Open Workspace](open-workspace.png)

:point_right: Now, back in the terminal (you may need to open a fresh one), at the shell prompt, move into the `projects/` directory and then use `git` to clone the repo (some typical output from the clone action is shown):

```
> cd $HOME/projects/
> git clone TODO-REPLACE-REPO-URL-HERE
Cloning into 'TODO-cloud-apis'...
remote: Enumerating objects: 158, done.
remote: Counting objects: 100% (158/158), done.
remote: Compressing objects: 100% (111/111), done.
remote: Total 158 (delta 53), reused 118 (delta 36), pack-reused 0
Receiving objects: 100% (158/158), 1.30 MiB | 3.26 MiB/s, done.
Resolving deltas: 100% (53/53), done.
>
```

> On opening up a new terminal after opening the `projects/` directory as the workspace, you may already have been put directly into the `projects/` directory, so the `cd $HOME/projects/` is not entirely necessary in this case (but it won't harm to run this command if you want to).

At this stage, you'll have the entire repo in your `projects/` directory, and you should see it in the Explorer area. It includes a directory called `workspace/` within the `exercises/03/` directory.

:point_right: Use the Explorer area to have a look around at the contents:

![Workspace directory contents](workspace-directory.png)

Here's a quick overview of what you see:

- `workflowproject/`: a directory containing a Multi-Target Application (MTA) which contains a workflow definition within a workflow module, that you'll be deploying to an instance of the Workflow service

- `authorities.json`: a JSON definition of scopes, or authorities, required when making some Workflow API calls

- `setup`: a simple script that uses the CF command-line client `cf` to set up an instance of the Workflow service, and also a service key

- `shared`: an include with shared variable definitions

- `workflow`: the main script that will help you make Workflow API calls


### 4. Create a Workflow service instance and service key

These steps assume you have a freshly created trial account on SAP Cloud Platform, and in particular, no existing Workflow service instance. If you do have such an instance already, you can either use that (and adapt the instructions here to suit) or remove it\* and follow the full instructions here.

> \*Only remove an existing instance if you have no more use for it.



## Summary

At this point you're ...

