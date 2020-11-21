`brew info zsh` 查看zsh信息

```ruby
✘ caven@CavendeMacBook-Pro ⮀ ~ ⮀ brew info zsh
zsh: stable 5.3.1 (bottled), HEAD
UNIX shell (command interpreter)
https://www.zsh.org/
Not installed
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/zsh.rb
==> Dependencies
Build: texi2html ✘
Required: gdbm ✘, pcre ✘
Optional: texi2html ✘
==> Options
--with-texi2html
 Build HTML documentation
--with-unicode9
 Build with Unicode 9 character width support
--without-etcdir
 Disable the reading of Zsh rc files in /etc
--HEAD
 Install HEAD version
caven@CavendeMacBook-Pro ⮀ ~ ⮀
```

升级`upgrade_oh_my_zsh` 时遇到问题：
```ruby
caven@CavendeMacBook-Pro ⮀ ~ ⮀ upgrade_oh_my_zsh
Updating Oh My Zsh
error: cannot pull with rebase: You have unstaged changes.
error: please commit or stash them.
There was an error updating. Try again later?
```


如何解决三步走：
`cd ~/.oh-my-zsh`

`git add .`

`git commit -m 'commit changes'`

`upgrade_oh_my_zsh`

[解决方法参考](https://github.com/robbyrussell/oh-my-zsh/issues/1991):

```ruby
caven@CavendeMacBook-Pro ⮀ ~ ⮀ cd ~/.oh-my-zsh/
 caven@CavendeMacBook-Pro ⮀ ~/.oh-my-zsh ⮀ ⭠ master± ⮀ git add .
 caven@CavendeMacBook-Pro ⮀ ~/.oh-my-zsh ⮀ ⭠ master± ⮀ git commit -m "commit changes"
[master 3db971b3] commit changes
 1 file changed, 115 insertions(+), 228 deletions(-)
 rewrite themes/agnoster.zsh-theme (60%)
 caven@CavendeMacBook-Pro ⮀ ~/.oh-my-zsh ⮀ ⭠ master ⮀ upgrade_oh_my_zsh
Updating Oh My Zsh
remote: Counting objects: 181, done.
remote: Total 181 (delta 69), reused 69 (delta 69), pack-reused 112
Receiving objects: 100% (181/181), 76.37 KiB | 32.00 KiB/s, done.
Resolving deltas: 100% (121/121), completed with 36 local objects.
From git://github.com/robbyrussell/oh-my-zsh
 * branch              master     -> FETCH_HEAD
   97c03841..66bae5a5  master     -> origin/master
 CONTRIBUTING.md                                               | 123 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 LICENSE.txt                                                   |   2 +-
 README.md                                                     |   4 ++--
 lib/completion.zsh                                            |   2 +-
 lib/functions.zsh                                             |   2 +-
 plugins/archlinux/README.md                                   |   2 ++
 plugins/archlinux/archlinux.plugin.zsh                        |   2 ++
 plugins/autoenv/autoenv.plugin.zsh                            |   2 +-
 plugins/battery/battery.plugin.zsh                            |   2 +-
 plugins/composer/composer.plugin.zsh                          |   5 ++++-
 plugins/docker/_docker                                        |   2 +-
 plugins/droplr/README.md                                      |   2 +-
 plugins/gnu-utils/gnu-utils.plugin.zsh                        |   2 +-
 plugins/gradle/gradle.plugin.zsh                              |  73 ++++++++++++++++++++++++++++++++++++++++++++++++++++---------------------
 plugins/history-substring-search/history-substring-search.zsh |   2 +-
 plugins/osx/osx.plugin.zsh                                    |  29 ++++++++++++++++++++++++-----
 plugins/scala/_scala                                          |   4 ++--
 plugins/suse/suse.plugin.zsh                                  |   4 ++--
 plugins/terraform/_terraform                                  |   2 +-
 plugins/ubuntu/readme.md                                      |   2 +-
 plugins/ubuntu/ubuntu.plugin.zsh                              |   2 +-
 plugins/web-search/web-search.plugin.zsh                      |   2 ++
 plugins/z/README                                              |   2 +-
 plugins/z/z.1                                                 |   2 +-
 themes/af-magic.zsh-theme                                     |   2 +-
 themes/bira.zsh-theme                                         |   2 +-
 themes/half-life.zsh-theme                                    |   2 +-
 themes/pure.zsh-theme                                         | 110 +++++++-------------------------------------------------------------------------------------------------------
 themes/refined.zsh-theme                                      | 106 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 themes/steeef.zsh-theme                                       |   2 +-
 themes/trapd00r.zsh-theme                                     |   4 ++--
 31 files changed, 348 insertions(+), 156 deletions(-)
 create mode 100644 CONTRIBUTING.md
 create mode 100644 themes/refined.zsh-theme
First, rewinding head to replay your work on top of it...
Applying: commit changes
         __                                     __
  ____  / /_     ____ ___  __  __   ____  _____/ /_
 / __ \/ __ \   / __ `__ \/ / / /  /_  / / ___/ __ \
/ /_/ / / / /  / / / / / / /_/ /    / /_(__  ) / / /
\____/_/ /_/  /_/ /_/ /_/\__, /    /___/____/_/ /_/
                        /____/
Hooray! Oh My Zsh has been updated and/or is at the current version.
To keep up on the latest news and updates, follow us on twitter: https://twitter.com/ohmyzsh
Get your Oh My Zsh swag at:  http://shop.planetargon.com/
 caven@CavendeMacBook-Pro ⮀ ~/.oh-my-zsh ⮀ ⭠ master ⮀
```
