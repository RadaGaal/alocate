alocate
=======

alocate (approximate locate) is an implementation of [`locate`][1] that can use
approximate regex matching to locate files.  This means you can locate files
named something close to what you remember, or with typos.  It is to `locate`
what [`agrep`][2] is to `grep`.

<https://github.com/chazomaticus/alocate>

About
-----

alocate is based on the excellent [mlocate][3] implementation of `locate`,
using [TRE][4]'s excellent approximate regex matching functionality.  In fact,
all I've done is replaced mlocate's regex matching with calls to TRE, and added
command line options to expose TRE's matching parameters.  This unfortunately
means you have to use `locate --max-cost=x --regex thing` or something equally
longwinded to see the effect.

I created the upstream branch from mlocate's mercurial repo
(<http://hg.fedorahosted.org/hg/mlocate>) using [git-hg][5].

Building
--------

I'm going to assume here that you're running Ubuntu, because I am.  mlocate is
already installed and set up on your system, which will let you easily run out
of the build directory.

Install the `libtre-dev` package.  You also may need `autopoint` and `libtool`,
if you didn't have them already.

Follow the instructions in `HACKING`.  It's more complicated than it should be,
but I couldn't get it to build any other way.

Run `configure`, pointing the `locate` binary to your existing database:

    ./configure --localstatedir=/var/lib

Then it should `make` without issue, and you can run `./locate` out of the
`src` directory without installing anything.

Running
-------

Since I've merely replaced mlocate's regex matching with calls to TRE, you need
to run `locate` with `--regexp` or `--regex` to invoke any approximate
matching.  The default max cost (how far off the matched file can be from the
supplied pattern) is 0, so you'll also want to pass `--max-cost` as something
higher than 0 to enable non-exact matches.

Here's a breakdown of the command line options I've added:
* `--cost-delete`, `--cost-insert`, `--cost-substitute` - set the costs of
  missing, extra, and wrong characters in filenames from the specified pattern
  (default for each is 1)
* `--max-cost` - set the maximum cost for a filename to be considered a match
  of the specified pattern (default is 0, meaning exact match)
* `--show-cost` - print the cost and a `:` before each matched filename

Future Plans
------------

There's probably lots more that could be done to make this much more useful.
Right now it's mostly a proof of concept.  See alocate's
[issues](https://github.com/chazomaticus/alocate/issues) for some ideas.

That said, I don't think I'll be actively working on this any time in the near
future.  I hope you find it useful as a base for something great, though.


Enjoy!


[1]: http://linux.die.net/man/1/locate
[2]: http://linux.die.net/man/1/agrep
[3]: https://fedorahosted.org/mlocate/
[4]: http://laurikari.net/tre/
[5]: https://github.com/cosmin/git-hg
