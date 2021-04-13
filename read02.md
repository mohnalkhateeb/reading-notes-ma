# **Git Introduction**
## *this page explain the Seeing Your Remotes section of git tool* 

# Seeing Your Remotes:
By running the ***git remote*** command, you can view the short names, such as “origin,” of all specified remote handles.

By using ***git remote -v***, you can view all the remote URLs next to their corresponding short names.

* # ***git remote*** Commands
  ## 1- Adding Remotes
  To create a new remote Git repository with a short name, use the following format: ***git remote add shortname url***
  This addition of these remote and short names allows you to use shortnames for Git collaboration.



  ## 2- Fetching
  Fetching entails pulling data that you don’t have from a remote project.
  Here is the command format:***git fetch [remote-name]***


  ## 3- Pushing
 To push your changes “upstream” for sharing, you would use the following git push command format:***git push [remote-name][branch-name]***
 
 *This command pushes committed changes from your local “master” branch upstream to the “origin” server.*

 ## 4- Renaming/Removing Remotes
   ### 1-Rename
   To rename a remote’s short name, use the ***git remote rename*** command.

   ### 2-Remove
   To remove a remote for whatever reason (e.g., a contributor has left the team, the server has moved), simply use the ***git remote rm*** command.


## To learn More about Git 
[click her](https://blog.udemy.com/git-tutorial-a-comprehensive-guide/#7)