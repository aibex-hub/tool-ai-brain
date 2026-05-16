![Info Database](./etc/ai-brain.png)
--------------------------------------------------------------------------------

# AI-Brain — Helper and Maintenance Utility for AI-Brains

`ai-brain` is a small `bash`-based CLI that turns any directory tree into a
context-aware knowledge base for an AI assistant (currently
[Claude Code](https://docs.claude.com/en/docs/claude-code)). Knowledge is
organized hierarchically; only what lies on the path from the current working
directory to the filesystem root is activated — *passend und ausreichend,
ohne Überschuss*.


## Curl Installation Formula

In a `bash` shell with installed `curl` execute the following one-line command
to download and install `ai-brain`. Select a number from the list of potential
install directories (which are extracted from your PATH).

```sh
  HUB=https://raw.githubusercontent.com/ihux; \
      curl -s $HUB/tool-ai-brain/ihux/bin/ai-brain >~ai-brain; bash ~ai-brain -!
```


## Pre-Requisites

Pre-requisites for `ai-brain` are a `bash` environment (as supported by Linux,
Mac-OS and Windows/WSL), [Claude Code](https://docs.claude.com/en/docs/claude-code)
installed and authenticated, and the `ec` color-echo helper from the
[Bluccino toolchain](https://github.com/bluccino).

Availability of `tree` is helpful for following the tutorial (it lets you see
the directory structure you build) but not absolutely necessary. With `curl`
installed the section above shows an easy way to download and install
`ai-brain`. Alternatively, cloning the repository and copying `bin/ai-brain` to
a binary directory listed in `$PATH` will also do the job (`ai-brain` is
located in the repository's subfolder `bin`).

```sh
    $ git clone https://github.com/ihux/tool-ai-brain  # see tool-ai-brain/bin/ai-brain
```

Tool `ai-brain` provides a version self-check:

```sh
  $ ai-brain --version
  1.0.1
```


## What Is an AI-Brain?

An **AI-Brain** is a directory hierarchy that lets you organize personal or
project-specific context for an AI assistant without bloating every query with
the full body of knowledge. The hierarchy follows two simple conventions:

* The **root** of an AI-Brain is a directory whose name starts with `§` — for
  example `§Privat/` or `§AIbex/`.
* Any directory inside the root may carry an `@/` subfolder (the **masterspace**)
  containing `.md` files with context that should be activated whenever you work
  in or below that directory.

When a query is sent from inside an AI-Brain, a hook script (registered by
`ai-brain --setup`) walks up from the current working directory to the
filesystem root, collects every `@/`-folder along that path, and prepends
their `.md` contents to the query as additional context. Branches outside the
active path stay inactive.

The result is **path-activated context**: precisely what is relevant to the
current task, nothing more.

~~~
    NOTE: The theoretical motivation for path-activated context is the
    "Adäquanz-Hypothese": both too little and too much context degrade the
    quality of AI responses. AI-Brain operationalizes that hypothesis by
    making the active context a function of the current working directory.
~~~


## Quick Start

After installation, run the embedded step-by-step tutorial to set up a working
example AI-Brain in a couple of minutes:

```sh
  $ ai-brain --tutorial
```

The tutorial walks you through:

1. creating the example directory tree
2. inspecting it with `tree -a`
3. populating three masterspaces with example content (recipes and electronics
   as two unrelated topics)
4. re-inspecting the populated tree
5. installing the hook with `ai-brain --setup`
6. running a first probe query inside Claude Code
7. verifying the activation (direct hook probe + side-branch comparison)
8. (optional) removing the hook again with `ai-brain --cleanup`

All commands inside the tutorial are presented as copy-paste-ready bash
blocks.


## Commands

```
ai-brain --setup      # install hook into the nearest AI-Brain (§-root)
ai-brain --cleanup    # remove the hook
ai-brain --hook       # print masterspace context along cwd path
ai-brain --tutorial   # step-by-step setup walkthrough
ai-brain --help       # comprehensive help
ai-brain --version    # print version
ai-brain -!           # install ai-brain into a PATH directory
ai-brain -?           # brief usage
```

`ai-brain --setup` finds the nearest `§`-prefixed directory above the current
working directory and places a project-local `.claude/settings.json` there
that wires `ai-brain --hook` into Claude Code's `UserPromptSubmit` hook. The
global `~/.claude/settings.json` is **not** touched, so the hook only
activates when Claude Code is run from inside the AI-Brain.

See `ai-brain --help` for full per-command descriptions.


## Conventions

| Pattern         | Meaning                                                |
|-----------------|--------------------------------------------------------|
| `§<name>/`      | AI-Brain root                                          |
| `@/`            | masterspace (context files, recursively)               |
| `.claude/`      | configuration folder (project-local `settings.json`)   |


## License

Apache License 2.0 — see [LICENSE](./LICENSE).
