# Git Work Flow

**Git Workflow**

![](https://miro.medium.com/v2/resize:fit:1400/1*W1LPtxxrJ0J1cq_Pv_OWbQ.png)

Well, we have two parts to our, our local repo on our local machine and the remote repo which is stored in a place like GitHub. While Git workflow does not necessarily have sequential steps to follow, I think it is best explained in a somewhat ordered fashion.

First, we get into our project and make a new branch called `new_branch` which is widely-viewed as an excellent name. We are amazing developers and `new_branch` is now filled with amazing code. We are going to go to our terminal and `git add` our code. This action moves our code from the working tree, over to the staging area.

Once our code is there and we have not made any changes, we can `git commit` our code and move it from staging over to our local master. Our local master has all of `new_branch` within it.

Our team lead wants us to push our local master into the remote repository, so we `git push` our code from our local branch into the remote repo.

The team lead also wants to make sure to that we add changes our coworker has added to the remote repo. A developer may use the command `git fetch` to _fetch_ the branch that our coworker has added. At this point, we have access to this code in our local repo, but it does not exist in our code. From here, we would `git merge` or `git rebase` after inspecting changes and send that code to the working tree to be worked on.

Let’s pause. In the last question, I went over that the process of `git fetch` and `git merge` can be replaced by a single command `git pull` . This will bypass the middle step of allowing the developer to track incoming changes and add them directly to the local master and have the code ready at the working tree. If you know the incoming code well enough, this is the quickest option.

Lastly, we want to move into a new branch that isn’t `new_branch` . For this we would `git checkout` into a new or existing branch on our local. This command gives us the most local agility.

Keep working hard!
