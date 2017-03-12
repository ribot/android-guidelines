# Git Conventions

## Naming conventions in Git

***[todo: move to Naming Conventions file]***

Feature branches should be named by this pattern:

`issN_abc_issue_name`,

where `issN` -- Issue number (i. e. `iss4`), `abc` -- initials of surname, name and patronymic (i. e. for Ivanov 
Petr Alekseyevich it will be `ipa`), `issue_name` -- full Issue name.

Commit message header should not contain dot at the end.

Commit message body should contain list with included features and optional details and notes:
```
Commit message header

- Change one
- Change two
- Change three

Details about the features and notes.
```

## Branching

**Master** and **dev** branches are protected from direct force pushing, editing and deleting. 
For the new features merging there are the **Pull Requests** system: 
https://help.github.com/articles/about-pull-requests/ 

After the feature realisation, Issue Assignee should create Pull Request for merging his new feature branch into the 
**dev**. 

Feature branches should **NOT** contain any merge commits or other garbage. Activity inside of feature branches should 
be done only by rebasing.

Pull requests should be merged with `--no-ff merge`.

### Pull request merging

_**[git_diagram_feature_branching]**_

Pull request checklist:

1. Conflict free commit tree
1. Approve from the Code Reviewer

The First requirement for a branch merging via a Pull Request is a lack of mering conflicts. The Assignee should 
resolve all conflicts.

The Second -- is a approve from the Code Reviewer. Code review is needed for the high quality of any new code additions. 

After a Code Review, Assignee should fix all founded bugs, problems etc, and add corresponding fixes in the 
merging branch. Each one commit should contain only one atomic fix.

_**[git_diagram_fixes_applying]**_

Then the Assignee should request a new Code Review. If all works correct, fix-commits should be squashed to a 
corresponding feature commits with correct commit message and hashes.

Then Assignee request the last Code Review and grant approve from the Code Reviewer. 

After that the Assignee can merge his branch to the **dev** by pressing the button.

### Feature branch caring

_**[git_diagram_feature_branching]**_

The Assignee should rebase his branch atop of the **dev** branch after each **dev** changing. Also he should notify all 
participants of this Issue about the branch rebasing.

Participants should rebase their commits atop of updated feature branch.

Participants should be notified about creating Pull Requests and starting each Code Review.

## Aliases and tools

**Prettylog** is for nice looking full commit history showing. Add `--all` for remote branches including.
```
prettylog = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) 
- %C(yellow)%aD%C(reset) %C(dim yellow)(%ar)%C(reset)%C(auto)%+d%n %<(80,trunc)%C(white)%s%C(reset)%n 
%C(dim white)by %an%C(reset) %n'
```

**Prettybranches** is for nice looking only branches heads showing. Handy for observing the whole tree structure. 
It includes `--all`.
```
prettybranches = log --graph --abbrev-commit --decorate --simplify-by-decoration --all 
--format=format:'%C(bold blue)%h%C(reset) - %C(yellow)%aD%C(reset) 
%C(dim yellow)(%ar)%C(reset)%C(auto)%+d%n %<(80,trunc)'
```
