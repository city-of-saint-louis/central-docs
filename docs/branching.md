# Branching
Branching is an integral part of our workflow; it facilitates multiple independent lines of development
and reduces the risk of conflicts in a collaborative working environment. By packaging changes into 
focused feature branches, we ensure that the main branch is kept clean and stable and that all changes
are isolated and easy to review and test. 

## Branch Name Components
A branch name consist of two components: A standard prefix and a descriptive name. The prefix should
be separated from the branch name by a single `/` character:  
`prefix/descriptive-branch-name`

## Standard Prefixes
Prefixes help to quickly identify the purpose of a branch, and all branch names should begin with one of
the following standard prefixes:

| Prefix    | Description                                   | Example                        |
|-----------|-----------------------------------------------|--------------------------------|
| feature   | add new functionality                         | feature/add-backend-logging    |
| bugfix    | address a bug or issue                        | fix/handle-database-error      |
| hotfix    | address a bug or issue (direct push to main)  | hotfix/controller-infinite-loop|
| refactor  | modify or improve existing code               | refactor/service-layer         |
| chore     | update dependencies or prod env               | chore/update-dependencies      |
| docs      | update or add documentation                   | docs/update

## Descriptive Name
- The portion of the branch name following the standard prefix should briefly describe the branch's purpose.
- The branch name should be all lower case
- Separate words in the branch name with dashes to improve readability
- Branch names should not include special characters (other than the aforementioned dash or forward slash)


## Examples
!!! success "good branch names"

    Add search functionality to a navigation element:
    `feature/nav-search-function`

    Update package.json:
    `chore/update-dependencies`

    Apply an emergency production fix:
    `hotfix/error-handling-vulnerability`

!!! failure "bad branch names"

    Add search functionality to a navigation element:
    `AddSearch_toNav`

    Update package.json:
    `UPDATE THE DEPENDENCIES!!!`

    Apply an emergency production fix:
    `feature-fix-bug`