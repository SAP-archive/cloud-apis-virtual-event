# Exercise 03 - Calling Workflow APIs

In this exercise you'll set up Workflow Management services in your trial account, and make a few API calls to the Workflow service instance. You'll do all this from the comfort of a dev space in the SAP Business Application Studio (App Studio), details of which were listed in the [../prerequisites.md](prerequisites).


## Steps

After completing the steps in this exercise you'll feel some familiarity with using the Workflow APIs in the Cloud Foundry (CF) environment of your SAP Cloud Platform trial account.

### 1. Add tools to your App Studio dev space

Because you're awesome and you've already set up your dev space according to the [../prerequisites.md](prerequisites), with the "MTA Tools" and "Workflow Management" SAP extensions installed, you're almost ready to start. First though, as we'll be interacting with the Workflow API from the command line within your App Studio dev space and learning about it that way, we'll install a couple of tools there to help us.

**jq - a lightweight and flexible command-line JSON processor**

We'll be dealing with JSON in this exercise, and [`jq`](https://stedolan.github.io/jq/) is a great way to parse it and extract values, on the command line and in shell scripts. We need to download the appropriate executable to our dev space environment, and we can do this by looking for the download link and using that in our dev space's terminal.

:point_right: Open up a new terminal with menu path **Terminal → New Terminal** (or with keyboard shortcut `Ctrl-\``) and create a directory in which to put this locally installed executable:

```shell
> mkdir -p $HOME/.local/bin/
```

> Your shell prompt may look different, for example it might look like this: `user: user $`, but for simplicity and consistency, shell prompts will be shown simply as `>` throughout.

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


### 2. Set up the Workflow Management related artifacts

These steps assume you have a freshly created trial account on SAP Cloud Platform, and in particular, no existing Workflow service instance. If you do have such an instance already, you can either use that (and adapt the instructions here to suit) or remove it\* and follow the full instructions here.

> \*Only remove an existing instance if you have no more use for it.



## Summary

At this point you're ...
