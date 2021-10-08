# How to contribute code to Ansible

## Steps for creating a pull request (Ansible)
Creating and sending a pull request steps:

#### 1. Fork your target repository:
1. Access to the [upstream](https://github.com/ansible/ansible/)
1. Fork the repository to your own organization([upstream](https://github.com/ansible/ansible/) > Press **fork**)
1. If you already forked the target repository, you probably need to fetch upstream changes(your repository > Press **Fetch upstream** > Press **Fetch and merge**)

#### 2. Clone your own repository and configure your name and email address:
```shell
$ git clone git@github.com:<YOUR_GITHUB_USER>/ansible.git
$ cd ansible
$ git config --local user.name "YOUR_FILL_NAME"
$ git config --local user.email "YOUR_EMAIL_ADDRESS"
$ git config -l      # You can check your user.name and user.email address settings.
```

#### 3. Add upstream repository as a remote repository:
```shell
$ git remote -v
origin  git@github.com:<YOUR_GITHUB_USER>/ansible.git (fetch)
origin  git@github.com:<YOUR_GITHUB_USER>/ansible.git (push)
$ git remote add upstream https://github.com/ansible/ansible.git
$ git remote -v
origin  git@github.com:<YOUR_GITHUB_USER>/ansible.git (fetch)
origin  git@github.com:<YOUR_GITHUB_USER>/ansible.git (push)
upstream        https://github.com/ansible/ansible.git (fetch)
upstream        https://github.com/ansible/ansible.git (push)
```
#### 4. Create a topic branch for your pull-request:

[a] Create a topic branch(my_test_branch) form current devel branch:
```
$ git checkout -b my_test_branch devel
Switched to a new branch 'my_test_branch'
$ git branch
  devel
* my_test_branch
```

[b] Create a topic branch(my_test_branch_for_backporting) from particular remote branch for back-port pull-request:
```shell
$ git branch -a
* devel
  remotes/origin/HEAD -> origin/devel
...snip...
  remotes/origin/stable-2.9      # HERE: target branch
...snip...
$ git checkout -b my_test_branch_for_backporting remotes/origin/stable-2.9
Updating files: 100% (19033/19033), done.
Branch 'my_test_branch_for_backporting' set up to track remote branch 'stable-2.9' from 'origin'.
Switched to a new branch 'my_test_branch_for_backporting'
$ git branch
  devel
* my_test_branch_for_backporting
```

#### 5. Happy coding time!(*Don't forget creating changelog fragments file... [See more details](https://docs.ansible.com/ansible/latest/community/development_process.html#changelogs)*)

#### 6. Add and commit your code:
```shell
$ git add <YOUR_NEW_FILEPATH>       # If you add new files.
$ git commit -a --signoff
```

#### 7. Push the changes to origin(your own repository):
```shell
$ git push origin HEAD
```

#### 8. Send a [draft pull-request](https://github.blog/2019-02-14-introducing-draft-pull-requests/) to upstream repository:
1. Access to the [Upstream](https://github.com/ansible/ansible/)
1. You will see **Compare & pull request** green button and press it
1. Set the destination branch(If your pull-request is back-port pull-request, you need to set specific target branch like stable-2.9)
1. Fill the title of your pull-request and description section
1. Turn **Allow edits by maintainers** on
1. Select **Draft pull request** on the green select box, then push it

#### 9. Confirm the results of CI tests on your [pull-request](https://github.com/ansible/ansible/pulls)

#### 10. If it failed with some errors, you need to modify the code and push it again:
Fix your code and commit it again:
```shell
$ git commit -a --signoff
```

Then, you should aggregate commit(You need to replace *pick* in 2nd line to *s* like below)
```shell
$ git rebase -i HEAD~2
--- BEGIN: example ---
pick ca4a8ae581 Do Not Merge - This is a test pull-request confirming CI tests
s abf84a85c1 HOGEHOGE

# Rebase bfa9c5d4b7..abf84a85c1 onto bfa9c5d4b7 (2 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
"~/work/src/saito-hideki/tmp/ansible/.git/rebase-merge/git-rebase-todo" 27L, 1205C
--- END: example---
```

Next step, you need to aggregate commit message as well:
```shell
--- BEGIN: example---
Do Not Merge - This is a test pull-request confirming CI tests

Signed-off-by: Hideki Saito <saito@fgrep.org>

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Fri Oct 8 13:23:28 2021 +0900
#
# interactive rebase in progress; onto bfa9c5d4b7
# Last commands done (2 commands done):
#    pick ca4a8ae581 Do Not Merge - This is a test pull-request confirming CI tests
#    squash abf84a85c1 HOGEHOGE
# No commands remaining.
# You are currently rebasing branch 'my_test_branch_for_backporting' on 'bfa9c5d4b7'.
#
# Changes to be committed:
#       new file:   changelogs/fragments/DNM_confirm_CI_tests.yml
#       modified:   lib/ansible/modules/system/hostname.py
#
--- END: example ---
```

Finally, you can force push the fix:
```shell
$ git push -f origin HEAD
```
#### 11. Ready for review
If your pull-request has passed all CI tests, you can set **[ready for review](https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests#draft-pull-requests)** on your pull-request page.

#### 12. Address to the change request
If you have a change request from a reviewer, you need to address this request. You can do this as the same as **Step 10**

## References
- [Ansible Community Guide](https://docs.ansible.com/ansible/latest/community/index.html)
- [The Ansible Development Cycle](https://docs.ansible.com/ansible/latest/community/development_process.html#the-ansible-development-cycle)
- [Rebasing a pull request](https://docs.ansible.com/ansible/2.9/dev_guide/developing_rebasing.html)
- [Developing collections](https://docs.ansible.com/ansible/2.9/dev_guide/developing_collections.html)
- [Ansible Collections Overview](https://github.com/ansible-collections/overview/#ansible-collections-overview)
