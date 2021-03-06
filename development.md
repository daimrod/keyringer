[[!meta title="Keyringer: development guidelines and workflow"]]

Index
-----

[[!toc levels=4]]

Coding standards
----------------

* Uses Semantic Versioning.
* Respect the existing coding style.
* Be clear: easy audability must be one of keyringer's requirements.

Release workflow
----------------

Go to develop branch and start a new release

    git checkout develop

Prepare the source code:

    $EDITOR keyringer # and update KEYRINGER_VERSION
    $EDITOR ChangeLog
    VERSION="`./keyringer | head -n 1 | cut -d ' ' -f 2`"

Create and upload a new release:

    make release

Tag the release:

    git tag -s $VERSION -m "Keyringer $VERSION"

Update the debian branch:

    make debian

Push everything:

    git push --tags

Build the package from the debian Git branch:

    git-buildpackage

Run lintian (or [add it to your pbuilder hooks](http://askubuntu.com/questions/140697/how-do-i-run-lintian-from-pbuilder-dist)):

    lintian --info --display-info --pedantic --color auto build-area/keyringer_$VERSION*.changes

Then go back to the develop branch and push everything:

    git checkout develop
    git push --all

Cleanup symlink:

    rm ../keyringer_$VERSION.orig.tar.bz2

Notes:

* `git-import-orig` takes care of running `pristine-tar commit`, of merging of the tag and orig tarball into the upstream branch, and then it merges the result into the debian branch. With the above configuration, it also runs git-dch to do the bulk of the work in `debian/changelog`.
* To build a development package, checkout the debian branch, merge master, run `git-dch --auto --snapshot` and build.

Packaging workflow
------------------

We recommend [this packaging workflow](https://git.sarava.org/?p=debian.git;a=blob;f=README.md;hb=HEAD).

Adding or changing a subcommand
-------------------------------

When adding a new subcommand or changing subcommand behavior, ensure:

* Documentation is updated.
* Manpage is updated.
* Shell completions are updated.

Test environment
----------------

Setup:

    keyringer test init ~/temp/tests/keyringer

Teardown:

    keyringer test teardown -y

Translation
-----------

Run just once:

    cd share/man
    po4a-gettextize -f text -m keyringer.1.mdwn -p keyringer.pot

References
----------

* [Using Git for Debian Packaging](http://www.eyrie.org/~eagle/notes/debian/git.html).
* [Building packages from the Git repository](http://honk.sigxcpu.org/projects/git-buildpackage/manual-html/gbp.building.html).
* [Cowbuilder](https://wiki.debian.org/cowbuilder).
* [git-pbuilder](https://wiki.debian.org/git-pbuilder).
* [PackagingWithGit - Debian Wiki](https://wiki.debian.org/PackagingWithGit).
* [Generating pristine tarballs from git repositories](http://joeyh.name/blog/entry/generating_pristine_tarballs_from_git_repositories/).
* [Debian Packaging](https://wiki.debian.org/Packaging).
* [Debian Upstream Guide](https://wiki.debian.org/UpstreamGuide).
* [DanielKahnGillmor/preferred_packaging - Debian Wiki](https://wiki.debian.org/DanielKahnGillmor/preferred_packaging).
