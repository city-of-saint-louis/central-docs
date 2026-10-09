# Workflow

### Overview
Our workflow is an essential part of our functional collaborative environment, and can be broadly categorized into three stages: [Implementation](#implementation), [Review](#review), and [Integration](#integration). 

Each workflow stage consists of one or more activities that must be completed before moving to the next stage of the workflow.  
___
### Implementation
#### Code
The developer makes code changes in their local development environment
#### Test
The developer tests code changes in their local development environment and writes tests that cover their work. 
#### Push
The developer uploads changes to a feature branch in the remote repository
___
### Review
#### Create Pull Request 
The developer creates a new [pull request](pull-request.md)
#### Request Review
The developer requests a [code review](code-review.md) from one or more other developers
#### Conduct Review 
The reviewers review changes made to the code and either approve them or request additional changes. CI Checks must pass.
___
### Integration
#### Merge
The developer resolves any conflicts with the main branch, then merges the code. If resolving conflicts requires significant changes, request another review first.
#### Deploy
Changes are deployed to the target environment
#### Verify
The developer confirms the changes work as expected in the target environment

___
!!! info "Iterative Process"
    Most workflow stages and activities are iterative. A common example of this iteration is as follows:

    - A developer completes initial implementation activities, creates a pull request, and requests a review.  
    - The reviewer conducts a review and requests that changes should be made to the code. 
    - The developer again performs the implementation steps and requests another review. 
    - The reviewer either approves this new round of changes or indicates that more changes are needed. 
    - This cycling of the implementation and review stages (and their associated activities) will continue until the changes have been approved and the workflow can advance to the integration stage.