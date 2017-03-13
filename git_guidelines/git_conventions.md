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

Pull request name should starts from "Request to..." 

## Branching

```
                                    (C1) 
                                     |
                                     |
                                    (C2)
                                     |
                                     |
                                    (C3) master
                                   /
                                  /
                              (C4)
                             / |
                            /  |
                        (C7)  (C5) dev 
                         |        \
                         |         \ 
                        (C8)        (C10)
                         |           |
                         |           |
   iss2_abc_feature_two (C9)        (C11)
                                     |
                                     |
                                    (C12) iss1_cfg_feature_one
```

The **master** branch contain only products releases.

The **dev** branch contain only feature merges.

The **master** and **dev** branches are protected from direct force pushing, editing and deleting. 
For the new features merging there are the **Pull Requests** system: 
https://help.github.com/articles/about-pull-requests/ 

After the feature realisation, Issue Assignee should create Pull Request for merging his new feature branch into the 
**dev**. 

Feature branches should **NOT** contain any merge commits or other garbage. Activity inside of feature branches should 
be done only by rebasing.

Pull requests should be merged with `--no-ff merge`.

### Pull request merging

```
                              (C4)
                             / |
                            /  |
                        (C6)  (C5) dev 
                         |        \
                         |         \ 
   iss1_cfg_feature_one (C7)        (C6')
                         |           |
                         |           |
   iss1_abc_feature_one (C8)        (C7')
                                     |
                                     |
                                    (C9) origin/iss1_cfg_feature_one
```

Pull request checklist:

1. Conflict free commit tree
1. Approve from the Code Reviewer

The First requirement for a branch merging via a Pull Request is a lack of mering conflicts. The Assignee should 
resolve all conflicts.

The Second -- is a approve from the Code Reviewer. Code review is needed for the high quality of any new code additions. 
 
```
                               (C4) dev
                              / 
                             /  
                         (C5)  
                          |     
                          |     
                         (C6)   
                          |     
                          |     
    iss1_abc_feature_one (C7)   
--------------------------------------------
                                Code Review
```

After a Code Review, Assignee should fix all founded bugs, problems etc, and add corresponding fixes in the 
merging branch. Each one commit should contain only one atomic fix.

```
                               (C4) dev
                              / 
                             /  
                         (C5)  
                          |     
                          |     
                         (C6)   
                          |     
                          |     
                         (C7)   
--------------------------------------------
                          |     Code Review
                         (C8)  \
                          |     | Fixes for C6
                         (C9)  /
                          |
                         (C10) Fix for C7
                          |
                         (C11) \
                          |     | Fixes for C5
                         (C12) /
                          |
    iss1_abc_feature_one (C13) Fix for C7
--------------------------------------------
                                Code Review
```

Then the Assignee should request a new Code Review. If all works correct, fix-commits should be squashed to a 
corresponding feature commits with correct commit message and hashes.

```
C5 commit message header

Commit message body.

Contain C11 fix (C11 hash)
    C11 message
Contain C12 fix (C12 hash)
    C12 message
```

```
                                (C4) dev
                               / 
                              /  
                         (C5')  
                          |     
                          |     
                         (C6')   
                          |     
                          |     
    iss1_abc_feature_one (C7')   
--------------------------------------------
                                Code Review
```

Then Assignee request the last Code Review and grant approve from the Code Reviewer. 

```
                                (C4) 
                               / |
                              /  |
                         (C5')   |
                          |      |
                          |      |
                         (C6')   |
                          |      |
                          |      |
    iss1_abc_feature_one (C7')   |
--------------------------------------------
                          |     Code Review
                           \     |
                            '---(C8) dev
```

After that the Assignee can merge his branch to the **dev** by pressing the button.

### Feature branch caring

```
                              (C4)
                             / |
                            /  |
                        (C6)  (C5) dev 
                         |        \
                         |         \ 
   iss1_cfg_feature_one (C7)        (C6')
                         |           |
                         |           |
   iss1_abc_feature_one (C8)        (C7')
                                     |
                                     |
                                    (C9) origin/iss1_cfg_feature_one
```

The Assignee should rebase his branch atop of the **dev** branch after each **dev** changing. Also he should notify all 
participants of this Issue about the branch rebasing.

Participants should rebase their commits atop of updated feature branch.

```
                              (C4)
                             / |
                            /  |
                        (C6)  (C5) dev 
                         |        \
                         |         \ 
                        (C7)        (C6')
                         |           |
                         |           |
                        (C8)        (C7')
                                     |
                                     |
                                    (C9) iss1_cfg_feature_one, origin/iss1_cfg_feature_one
                                     |
                                     |
                                    (C8') iss1_abc_feature_one
```

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

## GitHub conventions

### Pull requests

The Pull Request message should contain list with commits headers and hashes:

```
Pull Request #15
- Base-file spliting b9863c5
- Feature one 7f856c5
- Feature two 8a3b57c
```

Also it should contain links to corresponding resolving Issues for it automatic closing:
 
```
Pull Request #15
- Base-file spliting b9863c5
- Feature one 7f856c5
- Feature two 8a3b57c

Fixes #5
Fixes #7 
```
https://github.com/blog/1506-closing-issues-via-pull-requests

Merge-commit message should contain list of all branch commits:

```
Merge pull request #15 from User/iss14_abc_feature_branch_name
- Base-file spliting b9863c5
- Feature one 7f856c5
- Feature two 8a3b57c
- Additional feature 6a6c7b64
- Feature three 837f7c43
```

Every Pull Request should be assigned only on **one** Assignee. The Assignee presses Merge Button after approve 
receiving. After the merging the feature branch can be deleted. Issue should be closed. 

Issue and Pull Request should be moved to **Done** Project column.

### Projects

Participants should use project and cards to show their work status.

Done    | Do    | Will do   | Future concepts
---     |---    |---        |---
#13     |#5     |#1         |#5
#8      |       |#15        |#15
#3      |       |#3         |

**Done** contains done Issues and Pull Requests. **Do** contains on-progress things. **Will do** contain 
important Issues for near future work. **Future concepts** contains disputed and discussed things.

Issues and related Pull Request moving in Done only after full completing and merging.

### Links

Issues and comments can contain links to project files and commits if necessary:
https://guides.github.com/features/mastering-markdown/#GitHub-flavored-markdown
