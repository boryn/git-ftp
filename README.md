git-ftp.py: quick and efficient publishing of Git repositories over FTP
=======================================================================

Introduction
------------

Some web hosts only give you FTP access to the hosting space, but
you would still like to use Git to version the contents of your
directory.  You could upload a full tarball of your website every
time you update but that's wasteful.  git-ftp.py only uploads the
files that changed.

Requirements: [git-python 3.x](http://gitorious.org/git-python)  
it can be installed with `easy_install gitpython`

Usage: `python git-ftp.py`

Note: If you run git-ftp.py for the first time on an existing project 
you should upload to the hosting server a `git-rev.txt` file containing 
SHA1 of the last commit which is already present there. Otherwise git-ftp.py 
will upload and overwite the whole project which is not necessary.

Storing the FTP credentials
---------------------------

You can place FTP credentials in `.git/ftpdata`, as such:

    [master]
    username=me
    password=s00perP4zzw0rd
    hostname=ftp.hostname.com
    remotepath=/htdocs
    
    [staging]
    username=me
    password=s00perP4zzw0rd
    hostname=ftp.hostname.com
    remotepath=/htdocs/staging

Each section corresponds to a git branch.

Using a bare repository as a proxy
----------------------------------

An additional script post-receive is provided to allow a central bare repository
to act as a proxy between the git users and the ftp server.  
Pushing on branches that don't have an entry in the `ftpdata` configuration file
will have the default git behavior (`git-ftp.py` doesn't get called).
One advantage is that **users do not get to know the ftp credentials** (perfect for interns).  
This is how the workflow looks like:

    User1 --+                          +--> FTP_staging
             \                        /
    User2 -----> Git bare repository -----> FTP_master
             /                        \
    User3 --+                          +--> FTP_dev

This is how the setup looks like (One `ftpdata` configuration file, and a symlink to the update hook):

    root@server:/path-to-repo/repo.git# ls
    HEAD  ORIG_HEAD  branches  config  description  ftpdata  hooks  info  objects  packed-refs  refs
    root@server:/path-to-repo/repo.git# ls hooks -l
    total 0
    lrwxrwxrwx 1 root    root      29 Aug 19 17:17 update -> /path-to-git-ftp/update-hook


License
--------

Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.
