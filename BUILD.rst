Creating releases
=================

1. Check that master is up-to-date::

    git st
    git pull
    git push

2. Re-generate results available online if tests have changed or logs/reports
   have been enhanced::

    rm -rf results log.html report.html output.xml
    robot --version   # Make sure latest version is used
    robot --outputdir results QuickStart.rst
    git checkout gh-pages
    mv -f results/*.* .
    rmdir results
    git commit -a -m "Updated results available online"
    git push
    git checkout master

3. Create tag::

    VERSION=x.y    # Set this!!
    git tag -a -m "Release $VERSION" $VERSION
    git push --tags

4. Create release at https://github.com/robotframework/QuickStartGuide/releases

5. Announce on Twitter, mailing lists, and elsewhere as needed.
