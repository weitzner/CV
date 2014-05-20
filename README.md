*Curriculum vitae*
==

This is a repository containing the LaTeX document for my CV.

Formatting guidelines
---------------------
* Each sentence should begin on a new line
* Each author should use his/her own branch namespaced with the section s/he is working on
  * Good examples of branch names are "jane/abstract" and "roland/discussion"

Editing Tools
-------------
* MacTeX & TeXShop (http://mirror.ctan.org/systems/mac/mactex/MacTeX.pkg)
  * This download is about 2.25 GB and includes all of the TeX tools you will need including LaTeX, BibTeX and TeXShop.
* Familiarity with git will be helpful for tracking the progress of this document and visualization tools (e.g. SourceTree for OS X) may be very helpful. 

Visualizing the progression of the paper
----------------------------------------
* To see the difference in the raw TeX, use `git diff --color-words A..B`, where A and B two revisions you'd like to compare.  Remember that branch names are simply pointers to revisions, so this command `git diff --color-words master..abstract` will show the difference between the master branch and the abstract branch.  If these arguments are left out, the working directory will diffed against the most recent revision.
* To see the difference in the formatted output, you'll want to use latexdiff (/usr/texbin/latexdiff)
  * latexdiff is a perl script that generates a tex file summarizing the changes between two input tex files. That tex file can then be used to generate a pdf that shows these differences in real layout.
* To make this work nicely with git, you may want to add the following script at ~/bin/git-latexdiff

```sh
#!/bin/bash
TMPDIR=$(mktemp -d ~/git-latexdiff.XXXXXX)
latexdiff "$1" "$2" > $TMPDIR/diff.tex
pdflatex -interaction nonstopmode -output-directory $TMPDIR $TMPDIR/diff.tex
open $TMPDIR/diff.pdf
sleep 1
rm -rf $TMPDIR
```
**NOTE: this script assumes that you have added /usr/texbin to your PATH.**
* Now add the following stanzas to ~/.gitconfig or repo/.git/config

```sh
[difftool.latex]
        cmd = ~/bin/git-latexdiff "$LOCAL" "$REMOTE"
[difftool]
        prompt = false
[alias]
        ldiff = difftool -t latex
```

* You can generate a typeset pdf of the diffs by typing `git ldiff A..B cv.tex`

