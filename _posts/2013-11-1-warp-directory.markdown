---
layout: post
title:  "Warp Directory - because cd seems inefficient"
date:   2013-11-1 10:07:55
categories: shell utility
---

I was reading an article at [Hacker News][HN] on a small shell script called [bd][bd] (back directory) that let you quickly go back in directories, without having to type tedious sets of `cd ../../`. It was a clever idea that become relaitvely popular on HN. Then a buddy of mine had an idea that it would be awesome to have a script that let you jump to your often used directories, without having to type long pathnames like `cd ~/dev/project/name/blah` etc. So, we started hacking away at a Ruby script.

The initial idea was to save paths in a hidden config file in your home directory, which had a short name, like `dev:~/dev`. Then you could load those values into a hash with name as key and path as value. The script could then be run with `script dev` and boom, your shells working directory would be `~/dev`. After having implemented this we tested and nothing happend to the shells working directory. Then, after some headbutting, we realized that this of only worked for the environment which the script was run in, being some Ruby process. This would neither work with a shell script as they are run in a sub shell. But, then I thought that it might work if the script was included directly in the shell, like `. script args` or `source script args`, instead of actually running it. So with some minor configuration we had something that worked. This, though, brought some challenges to error handling and variables as a call to `exit` would actually exit your working shell, as well as variables and functions could overwrite important functions or environment variables. So, with some garbage collection and funky function names we worked around it.

With [getopt][getopt] I have added features such as `add` which taken one parameter, the key of the "warp point". The script then takes the `PWD` environment variable and stores that with the key in the config file. If you run it with with an exclamation mark, like `add!`, it overwrites one previously added with same name. Then the `rm` argument does the, but removes it and `ls` lists all warp points.

So to make the script a little nicer to run, not having to write the `.` or `source` infront of the script, adding a little `alias wd='. script` would do the trick. So now the working implementation can be found at [GitHub][wd]. The code is currently at 200 lines so isn't a too long of a read. If you have some improvements or features to add, send me a pull request. I'm addings things as they come up.

[HN]: https://news.ycombinator.com/item?id=6309639 "Hacker News"
[bd]: https://github.com/vigneshwaranr/bd "Back Directory"
[wd]: https://github.com/mfaerevaag/wd "Warp Directory"
[getopt]: http://en.wikipedia.org/wiki/Getopt#Shell "Getopt"
