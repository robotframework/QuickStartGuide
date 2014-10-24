Creating releases
=================

1. Re-generate results available online if tests have changed::

    rm -rf results log.html report.html output.xml
    pybot --outputdir results QuickStart.rst
    git checkout gh-pages
    mv -f results/*.* .
    rmdir results
    git commit -a -m "Updated results available online"
    git checkout master
    git push

2. Create tag::

    VERSION=x.y    # Set this!!
    git tag -a -m "Release $VERSION" $VERSION
    git push --tags

3. Create release at https://github.com/robotframework/QuickStartGuide/releases

4. Announce on Twitter, mailing lists, and elsewhere as needed.
