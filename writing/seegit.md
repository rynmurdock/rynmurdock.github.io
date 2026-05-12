---
title: "Generative Recommenders"
date: 2026-05-12
tags:
  - rite
  - writing
---

_May 12th, 2026_

---

## seegit: trace & rebuild your files

## tl;dr

I've written a python tool for "tracing" and "rebuilding" commits from git repos: https://github.com/rynmurdock/seerite

## Details

Using this package, you can take a git repo, selectively store each file's diffs in a dataframe, and recreate files up to specific diffs in their history. We refer to these as "tracing" and "rebuilding" respectively.

seegit may be especially useful in combination with [dura](https://github.com/tkellogg/dura/) and autosaving in your editor of choice ([e.g. using vscode](https://medium.com/@ssjewcw/dev-tips-vscode-auto-save-7ed88d31363e)). Together they can be used to track (dura) each moment-by-moment change (autosave) followed by tracing/rebuilding (seegit) changes to files. I could imagine this being used for research on people's typical writing processes, for understanding changes to files of many formats outside of text, or for creating statistics tracking how an artistic process evolves and in what ways.

<img src="/images/seegit-screenshot.png" alt="A README file's diffs. 'Select your diff to load.'" style="max-width:500px; width:100%;" />


## My reason for writing the tool

I've wanted to model the edit "trajectories" of my short-form writing, in this case specifically by conditioning on a given state of a text and predicting a patch to that text that would move it closer towards a finished text. I was surprised that while there are packages for exporting the diffs of files in a git repo, rebuilding seemed to not be available and no packages fit my purposes well. 

This modeling branch of the project has been woefully unsuccessful. Likely due to a combination of:

- immature writing on my part -- I cringe at almost every line of edits, which seem to make caricatures of any phrases I use.
- too little writing on my part -- my N of finished files is in the dozens.
- mistakes in trajectories are collected as much as good edits! I have some plans for how to mitigate this :)


## Conclusion

I hope folks find this tool useful; please leave feedback in issues, contributions in PRs, and uh I guess wonderful edits in your hearts :D

