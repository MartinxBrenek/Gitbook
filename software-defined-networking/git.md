# Git

**Git** is a version-control system that turns every change into a small, inspectable unit of work. You work on files that represent intent. You ask for a review, and then you merge the new changes with the previous ones.

A Git version control system is most effective for network change tracking when it is used to manage intent rather than outputs. You can store YAML Ain't Markup Language (YAML) or comma-separated values (CSV) inventories, policy maps, Jinja2 templates, variables, Terraform HashiCorp Configuration Language (HCL), pyATS tests, and your CI pipeline files. Exclude secrets like passwords and keys, packet captures and images, large binaries, and Terraform state. Store those in a vault, artifact storage, a config-backup system, or a remote back-end (Terraform state) instead.

You can track your IaC sources the same way you track Cisco IOS XE Software fragments. Git versions plaintext reliably. Every change is reviewable and reversible.

* **Terraform:** Commit \*.tf, modules, variables, and terraform.lock.hcl to pin providers. Do not commit state files or the .terraform working directory.
* **Ansible:** Commit playbooks, roles, group\_vars, host\_vars, and inventory definitions. If you use Ansible Vault, version the encrypted files, never the vault password.
* **Templates and data:** Commit Jinja2 templates, YAML, or CSV inventories, JavaScript Object Notation (JSON) payloads for Representational State Transfer Configuration Protocol (RESTCONF), and small Python utilities for validation.

You can also store full device configurations in Git. This helps with audit trails and comparisons.

* Keep full device configuration snapshots separate from the network intent files. Use a dedicated **snapshots** folder or, better, a separate repository with a predictable structure like _snapshots/\<site>/\<device>/\<YYYY-MM-DD>/running-config.txt,_ to ensure that extensive diffs do not slow down the review process for network changes.
* Redact secrets or avoid committing them. Automate redaction with a pre-commit hook.
* Treat running configuration snapshots as read-only evidence. Make all network changes by editing the desired state in version-controlled IaC files, reviewed in merge requests and applied by automation. Keep device configuration or state as archived snapshots.

## Essential Git Operations for Tracking Network Changes <a href="#page-heading" id="page-heading"></a>

Here are the most common Git commands that include initializing a repository, adding files, and committing changes.

Choose each step of version control to discover these commands.

#### Initializing a Git Repository

The first step in version control is to initialize a repository. Navigate to the directory where your Python scripts are located and run:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git init
</code></pre></td></tr></tbody></table>

This command creates a .git subfolder, making the current folder a Git repository. After initializing, Git will start tracking the files in this directory.

#### Checking the Status

After you have edited some files in your repository, you can check which files have been modified using the following command:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git status
On branch main
Changes not staged for commit:
(use "git add &#x3C;file>..." to update what will be committed)
(use "git restore &#x3C;file>..." to discard changes in working directory)
modified:   configs/vlan_site_A.yml
modified:   configs/acl_site_A.yml
no changes added to commit (use "git add" and/or "git commit -a")
</code></pre></td></tr></tbody></table>

The section **Changes not staged for commit** tells you which files were modified and not yet staged.

If a file was already staged with the `add` command, then that file will be shown under the **Changes to be committed**.

#### Checking the Diff

Besides seeing modified files, you can also inspect the modifications. `git diff` shows unstaged changes. Lines with **`-`** will be removed, lines with `+` will be added.

In the following example, you renamed the Guest VLAN, and added VLAN 40.

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git diff
diff --git a/configs/vlan_site_A.yml b/configs/vlan_site_A.yml
index 3b2a9d1..7d8e1c4 100644
--- a/configs/vlan_site_A.yml
+++ b/configs/vlan_site_A.yml
@@ -3,6 +3,7 @@ branches:

id: site_A
vlans:

{ id: 20, name: Users }



 - { id: 30, name: Guest }




 - { id: 30, name: Guest-WiFi }


 - { id: 40, name: IoT }


</code></pre></td></tr></tbody></table>

#### Adding Files to the Repository Staging Area

After initializing the repository, you need to tell Git which files you want to track by adding them to a staging area. To add all files in the directory:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git add .
</code></pre></td></tr></tbody></table>

This command stages all the files in the current directory for the next commit. If you only want to add specific files, you can list them individually:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git add network_config.yml
</code></pre></td></tr></tbody></table>

#### Committing Changes

Once you have added the files, it is time to commit the changes. A commit is like taking a snapshot of your project at a particular point in time:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git commit -m "Initial commit of network configuration intent for Ansible"
</code></pre></td></tr></tbody></table>

The **-**`m` flag allows you to add a short message describing the changes that you have made. This message helps others (and yourself) understand the purpose of the commit.

Writing a good commit message can be very important. It clearly explains the purpose and context of a change, helping others (and yourself in the future) understand what was done and why.

How to write a good commit message?

* Start with a verb, such as "Add," "Fix," "Update," or "Remove."
* Explain what changed and why.
* Keep the message short but not too vague.
* Examples for network changes: "Add VLAN 30 for guest Wi-Fi network," "Fix ACL sequence conflict on engineering subnet," "Remove deprecated sales ACL rules per security audit."

#### Viewing Commit History

To view a history of all the commits in your repository, run:

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git log
commit 7cd6c22e5ebf1d46afe0c77cabaea5b015b12cb5 (HEAD -> main)
Author: Git User &#x3C;user@example.com>
Date:   Wed Oct 29 18:52:08 2025 +0100
Fix vlan bug
commit 9853262b88d1c78059b9a38dd00077a9d4c1e12f
&#x3C;... output omitted ...>
</code></pre></td></tr></tbody></table>

This displays a list of all commits, along with their unique commit IDs, dates, and messages.

`Note Git tracks changes by recording diffs between the current state of a file and its previous version, rather than saving complete copies of the entire file each time.`

## Collaboration and Remote Repositories

When working in teams, Git allows you to store your code in remote repositories (for example, on GitHub, GitLab, or Bitbucket). These platforms, often free but with premium versions available, allow you to collaborate with others, share your code or network configuration files, and synchronize changes.

Once you configure a remote repository URL with `git remote add`, you can use `git push` and `git pull` commands, as shown in the following figure.

You can use the following commands to manage remote and local repositories:

*   If you started a local repository with `git init,` you must add a remote URL yourself. First, create a new remote repository on GitLab (or another platform). Once your remote repository is created, you can link your local repository to it by running:

    ```
    git remote add origin https://gitlab.com/yourusername/your-repo.git
    ```

    Replace _yourusername_ and _your-repo_ with your actual GitLab username and repository name, and make sure that you are using the required authentication.
*   If a remote repository exists and you do not have a local copy, run the following command:

    ```
    git clone https://gitlab.example.com/yourusername/your-repo.git
    ```

    Replace _yourusername_ and _your-repo_ with your project. You can use the SSH form too, git@gitlab.example.com:yourusername/your-repo.git. This creates a local folder, sets the remote name to _origin_, and checks out the default branch, usually _main_. Then cd _your-repo_ and start working.

    If you need to switch from HTTPS to SSH, update the URL.

    ```
    git remote set-url origin git@gitlab.example.com:yourusername/your-repo.git
    ```
*   After committing your changes locally, you can push them to the remote repository:

    ```
    git push -u origin main
    ```

    The _main_ in this command stands for the branch name. If you are working with a different branch, you need to modify this part of the command.
*   If someone else has changed the repository, or you want to work from a different workstation, you can pull the remote changes to your local environment by running:

    ```
    git pull origin main
    ```

    This command fetches changes from the remote repository and merges them with your local version. If you want to only fetch changes without merging them to your local version, use `git fetch`.

{% hint style="info" %}
Why are we always using **origin** as the name of the remote repository when running Git commands?

Because `git clone` automatically creates a remote repository named **origin** for the URL you cloned. **origin** is just a label, it makes commands like `git push origin main` short and consistent. Most teams adhere to this convention to maintain simplicity in documentation, scripts, and CI processes.

You can change it or add more remotes

```
# rename if you prefer a different label
git remote rename origin gitlab

# fork flow, keep origin = your fork, add the source repo as 'upstream'
git remote add upstream git@git.example.com:net/infra-upstream.git
```
{% endhint %}

## Use Git Branches

A branch provides an isolated environment for managing a single change. You isolate the work, write small commits, open a review, and merge when approved. The _main_ branch stays clean and production ready. You can maintain many branches at the same time. Each new branch should address a single objective to ensure reviews remain concentrated and rollbacks are straightforward.

Name branches to make the purpose obvious. Include the scope, the device group or site, and an issue ID if you have one. Examples are _feature/guest-vlan-b01-b02, fix/qos-voice-b01, policy/acl-engineering-101, chore/inventory-cleanup-TEAMS-123._ Avoid spaces. Use short, descriptive names.<br>

For example, if you are working on changing the configuration file for VLANs, you can create a branch to make the changes, without affecting the main project.

*   Make sure you start from the branch that already contains the baseline you want to work from, usually the _main_.

    ```
    git branch

    * feature/guest-vlan-b01-b02
      main
      staging
      production
    ```

    If you intended to be on the main branch but are not currently on it, switch using the `git switch main` or `git checkout main` commands.
*   Once you are on the correct branch, sync to the latest changes. Others may have updated the main branch, so pull first to avoid merge conflicts later.

    ```
    # Fast-forward only, safest default

    git pull --ff-only
    ```

{% hint style="info" %}
The `--ff-only` flag makes sure that Git will update your branch only if it can move the branch pointer ahead without creating a merge commit. If your branch has diverged, Git stops and asks you to resolve it with a merge or a rebase. This keeps the commit history clean and predictable. In practice this happens when you made local commits on main, or you have uncommitted changes on main.
{% endhint %}

*   To create a new branch, use the following command:

    ```
    git checkout -b vlan-changes
    ```

    Alternately, you can use the following:

    ```
    git switch -c vlan-changes
    ```

    This creates and switches to a new branch named _vlan-changes_. Now, you can change the configuration files in this branch without affecting the source branch.

    You can create as many branches as needed to work on new features without affecting the _main_ branch. When you create a new branch, it starts from the branch you were currently on, inheriting its code and commits history.
*   Once you have completed your changes in the branch, you can merge it back into the _main_ branch (or any other branch). First, switch back to the main branch:

    ```
    git checkout main
    ```

    Then, merge the changes from your feature branch:

    ```
    git merge vlan-changes
    ```

    This integrates the changes from the _vlan-changes_ branch into the primary branch, typically named _main_.

Sometimes a merge conflict can occur. This happens when Git cannot combine changes automatically. For example, both branches edited the same lines, one branch deleted a file the other modified, or there are overlapping renames, and Git is unable to automatically determine which file version to retain.

For example, you try to merge _feature/acl-fix_ into the main branch, but it fails because new commits on _main_ changed the same lines in the same files as your _branch_.

```
git switch main
git pull
git merge feature/acl-fix

Auto-merging policies/acl_guest_iosxe.cfg
CONFLICT (content): Merge conflict in policies/acl_guest_iosxe.cfg
Automatic merge failed, fix conflicts and then commit the result.
```

{% hint style="info" %}
So what do those markers in a merge conflict mean?

* `<<<<<<< HEAD` marks the start of your version, representing the content from the currently checked-out branch. In this example, _main._
* `=======` separates the two conflicting versions.
* `>>>>>>> feature/acl-fix` ends with the other side, the incoming branch name that you want to merge from, and its content.
{% endhint %}

Pick the correct configuration lines, delete the unwanted configuration lines, including the markers, save, then `git add` and `git merge --continue` until there are no merge conflicts remaining.

If you decided that the changes coming from the **feature/acl-fix** branch should be merged, then keep those, and delete the lines that are currently in the main branch. Here's the modified file, ready to be merged.

```
# policies/acl_guest_iosxe.cfg
ip access-list extended GUEST_INTERNET
 10 permit tcp any any eq www
 20 deny tcp any any eq 443
 30 permit icmp any any echo-reply
```

You can continue the merge by staging the changed file again, and running the **merge** command with the **--continue** flag.

```
git add policies/acl_guest_iosxe.cfg
git merge --continue
```

{% hint style="info" %}
When you merge two branches, for example **main** and **feature**, each branch has its own list of commits, that list is its _history_. A regular merge makes a new commit that combines the changes from both lists, this new commit is placed at the end of the branch, the tip, which Git also calls **HEAD**. If Git can fast forward, meaning one branch is simply behind the other with no separate commits of its own, it does not create a new commit, it just moves the branch pointer so the tip, HEAD, points to the latest commit. In short, regular merge creates a new merge commit, fast forward just advances the pointer.

HEAD is your current spot in history, usually the latest commit on the branch you have checked out. On main, HEAD points to the latest on main. Switch branches and HEAD moves. Make a new commit and HEAD advances.

You are not restricted to CLI usage for Git operations. Visual tools make it easy to stage by hunk, view diffs, resolve conflicts, browse history, and open reviews. Popular choices include VS Code Source Control and the GitLens extension, JetBrains IDEs, GitHub Desktop, GitKraken, and Sourcetree.
{% endhint %}

Work in short cycles. Start from the latest _main_ branch, create a new branch, make a small change, push, and open a review. Delete the branch after merge. If you need to work on two unrelated changes at once, create two branches.

## Advanced Git Techniques for Efficient Change Control <a href="#page-heading" id="page-heading"></a>

* **Cherry-pick:** Copy an exact commit to another branch when only one fix needs to move forward.
* **Revert:** Undo an incorrect commit on a shared branch while keeping the history.
* **Reset:** Rewrite your local branch before pushing changes to remove unwanted commits or start fresh.
* **Restore:** Discard or recover a specific file from a known version.

The goal is a clean historical record for reviewers, safe network rollouts, and fast recovery when something goes wrong.

### Git Cherry-pick

On a feature branch, you often make several focused commits. Usually, you merge these custom branch changes into the _main_. If you need only one of those changes on another branch, use `git cherry-pick` to copy that exact commit without including the others.

Cherry-pick copies one specific commit onto your current branch. This is perfect for hotfixes that must appear in staging and production without merging unrelated work.

```
# On staging branch, bring in the exact fix from its commit on another branch
git switch staging
git cherry-pick <fix-commit-sha>

# On production, repeat after staging is green
git switch production
git cherry-pick <fix-commit-sha>
```

With `git cherry-pick`, you need to specify the commit SHA, which is the unique ID for a commit. To identify it, use the `git log` command.

```
# Identify the fix you want to move
git log -n 5

commit 7cd6c22e5ebf1d46afe0c77cabaea5b015b12cb5 (HEAD -> main)
Author: Git User <user@laptop>
Date:   Wed Oct 29 18:52:08 2025 +0100

    Fix ACL guest allow 443

commit 9853262b88d1c78059b9a38dd00077a9d4c1e12f
Author: Git User <user@laptop>
Date:   Mon Oct 27 15:54:57 2025 +0100

    Guest VLAN b01 b02
```

The additional flag `--oneline` concatenates the output to show only the commit SHA and commit message. The `-n` flag specifies the number of commits you want to display.

```
# Identify the fix you want to move
git log --oneline -n 5
7cd6c22 (HEAD -> main) Fix ACL guest allow 443
9853262 Guest VLAN b01 b02
...
```

As an example, the following cherry-picking procedure is used for a quick hotfix on a _testing_ branch, without merging unrelated work. The commit SHA _7cd6c22_ has the ACL changes that you want to introduce to another branch, in this case testin&#x67;**.**

The specific commit that you want to cherry-pick was done on another branch. However, the objective is not to merge all changes (commits) from that branch, but rather a particular one that addresses the immediate requirement.

```
# Apply that commit on testing branch
git switch testing
git pull --ff-only
git cherry-pick 7cd6c22

[testing 3c7b0b9] Fix ACL guest allow 443
  1 file changed, 1 insertion(+), 0 deletions(-)
```

### Git Reset

The following figure shows a `git reset` on a branch named _hotfix_, where changes from several commits were removed by referencing one of the commits in the history.

The `git reset` command moves your branch back to a chosen commit. What happens to your files depends on the mode. With `--hard`, Git also updates the index and working tree, discarding later commits, and local edits.

For example, you made 10 commits. The `git reset <first-sha>` command takes you back to the first commit and drops the nine that followed on your local branch. _Do not use this on a shared branch._

Have a look at more `git reset` examples using different reset modes.

Choose each reset mode to discover more about its function.

git reset --soft

The `--soft` mode aims to change the HEAD reference to a specific commit. For instance, if you realize that you forgot to add a file to the commit, you can move back using the `--soft` option with respect to the following format:

* Use `git reset --soft HEAD~n` to move back to the commit with a specific reference (n). For example, `git reset --soft HEAD~1` moves back to the last commit.
* Use `git reset --soft <commit ID>` to move back to the head with the _\<commit ID>_.

The last commit is located at the HEAD. You can fix this issue by running the following three statements.

* Return to the pre-commit phase using `git reset --soft HEAD`, which enables Git to reset the file.
* Add the forgotten file with `git add`.
* Make the changes with a final `git commit`.

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git reset --soft HEAD~1
git add vlan.yml
git commit -m "added the VLAN configuration file"
</code></pre></td></tr></tbody></table>

git reset --mixed

This is the default argument for `git reset`. Running this command has two impacts: it uncommits all the changes and unstages them. For example, if you accidentally added the **customerA\_vlan.yml** file, and you want to remove it.

* Unstage the files that were in the commit with `git reset HEAD`.
* Add only the files that you need for the commit.

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>git reset HEAD
git status
On branch master
Untracked files:
(use "git add &#x3C;file>..." to include in what will be committed)
customerA_vlan.yml
customerB_vlan.yml
customerC_vlan.yml
nothing added to commit but untracked files present (use "git add" to track)
git add customerB_vlan.yml  customerC_vlan.yml
git commit -m "Removed the customerA_vlan.yml from the commit"
</code></pre></td></tr></tbody></table>

git reset --hard

This option can be dangerous, so use it with caution. Performing a hard reset on a specific commit forces HEAD to return to that commit and deletes all changes made after it.

<table data-header-hidden><thead><tr><th></th></tr></thead><tbody><tr><td><pre><code>ls
customerA_vlan.yml  customerB_vlan.yml  customerC_vlan.yml 
git reset --hard 97159bc
HEAD is now at 97159bc added customer A and B VLAN
ls
customerA_vlan.yml customerB_vlan.yml
</code></pre></td></tr></tbody></table>

Notice in the example that the untracked file customerC\_vlan.yml is deleted. Again, ensure that you fully understand the effects of the `reset` command before using it.

### Git Revert

The following figure shows a `git revert` on a **hotfix** branch with three commits. You revert the first commit. Git creates a new commit that undoes the changes only from that first commit, leaving the other commits intact.

`git revert` creates a new commit that inverts a specific previous commit. It keeps history intact, which is why it is the safest way to undo on branches that others pull from.

For example, undo a faulty access list change on the main branch.

<pre><code>git switch main
git pull --ff-only
<strong>git revert &#x3C;bad-commit-sha>
</strong>git push
</code></pre>

### Git Restore

If you do want to revert only changes of a single file and not the whole commit(s), you can use a `git restore [--source=<ref>] <filepath>` . With `git restore`, no new commit is created until you make one.

```
# Discard local changes to a file
git restore policies/acl_guest_iosxe.cfg

# Restore a file to a prior version, then record that as a new commit
git restore --source=HEAD~1 -- policies/acl_guest_iosxe.cfg
git add policies/acl_guest_iosxe.cfg
git commit -m "Restore ACL file to HEAD~1 version"
```

{% hint style="info" %}
When should you use `git revert`, `git reset`, and `git restore`?

On shared branches, use `git revert`. For local history cleanup, use `git reset`. For discarding or restoring one file, use `git` `restore`.

* If you have already pushed your commits, `git reset` is risky because it rewrites history. Prefer `git revert`, which keeps all prior commits and adds a new commit that undoes the target, so the audit trail stays intact.
* Use `git reset` before you push to move your branch back to an earlier commit. Choose the mode based on what you want to keep. `--soft` keeps changes staged, `-`**`-mixed`** keeps them but unstages, `--hard` discards them. This lets you rewrite a message, split a big change into smaller commits, or start fresh.
* Use `git restore` to discard uncommitted edits in a file or to revert a file to a state from a specific commit, tag, or origin/main, then commit that change.
{% endhint %}

## Centralized Storage with GitLab <a href="#page-heading" id="page-heading"></a>

Both GitLab and GitHub host Git repositories and support reviews and automation. GitLab is often chosen for its all-in-one platform in a single product, including built-in CI runners, approvals, code owners, secret scanning, and detailed permissions. If your company standardizes on GitHub, the same ideas apply.

GitLab is a self-hosted or cloud-based platform that enables you to manage the full lifecycle of Git projects, from storage and reviews to CI/CD and releases, through an intuitive web interface.<br>

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

If you have a local Git repository that you want to manage with GitLab, you must first create a project on GitLab. Once the project is created, you can find the SSH or HTTP URL that you can use to configure your remote URL, or run a `git clone`.

Once you have the URL from the project repository, and you have a local Git repository already, you can configure the GitLab remote and push the changes.

```
git remote add origin git@gitlab.example.com:student/network-configs.git
git push -u origin main
```

Use the project page to browse commits and configuration changes, track issues, or open a merge request to merge changes from one branch into another using the web interface.

### GitLab CI/CD Pipeline

GitLab includes built-in CI/CD pipelines, so you automate lint, render, and plan steps in the same place you store your configurations, no extra tools needed.

### Note

When self-hosting GitLab, you need to deploy your own GitLab runners that will execute the pipeline jobs.

When configured, each merge request can run the pipeline, store artifacts, and can block the merge until checks pass. This keeps every change tested, consistent, and auditable before it reaches devices.

The GitLab pipeline consists of:

* **Commit:** A change in the code.
* **Job:** Runner instructions.
* **Pipeline:** A group of jobs divided into different stages.
* **Runner:** A server or agent that implements each job separately and can spin up or down if needed.
* **Stages:** Parts of a job (for example, build or tests). Multiple jobs inside the same stage are executed in parallel.

A GitLab CI/CD pipeline is configured using a YAML file (.gitlab-ci.yml) in the root of the project.

```
stages: [render, plan]

render_acl:
  stage: render
  image: alpine
  script:
    - mkdir -p rendered
    - echo "sample IOS-XE ACL" > rendered/acl.txt
  artifacts: { paths: [rendered/acl.txt] }

terraform_plan:
  stage: plan
  image: alpine
  script:
    - mkdir -p terraform
    - echo "sample terraform plan" > terraform/plan.txt
  artifacts: { paths: [terraform/plan.txt] }
```

The previous CI example configuration runs two jobs in order, **render\_acl** then **terraform\_plan**. Each job starts a clean Alpine container, creates a folder, writes a sample text file, and uploads it as an artifact. GitLab stores these artifacts with the pipeline, you can open them from the job page or the merged request to verify outputs.

The CI example is simple, and can be used for testing to confirm that your CI is wired correctly, stages run, artifacts persist, and permissions work. After you are sure that the pipeline runs as expected, you can replace the echo lines in the previous example with real template rendering and Terraform commands.<br>

Pipelines can be initiated by various events; you can choose triggers to suit specific workflows.

Common triggers are:

* Push to a branch
* Merge request created or updated
* New tag pushed
* A scheduled time
* A manual click
* An API trigger

You control this with rules in the GitLab CI configuration file. You can also skip a pipeline by putting `[skip ci]` in the commit message.

```
# Run only on merge requests
rules:
  - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

```
# Run when files in these folders change
rules:
  - changes:
      - policies/** 
      - terraform/**
```

## Gitbook - ​​What is documentation as code?

**Documentation as code** is the process of creating and maintaining documentation using the same tools that you use to code.

That could mean different things depending on the tools you use, but it will probably involve elements such as version control, Markdown formatting, automated reviews and tests. By following these existing workflows, the development and product teams can work more closely together, and technical writers can be involved in the documentation process earlier.

It also means that your technical documentation is easier to keep up-to-date, because it’s created in sync with the development process. It’s also likely to be more accurate, as the developers themselves will typically write the first draft themselves.

Plus, with the option to include documentation within those automated reviews and tests, you can catch any undocumented code before it’s merged, and check for formatting and style errors. It all adds up to documentation that’s clearer and more consistent.
