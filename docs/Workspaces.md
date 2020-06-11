# Laminar workspaces

Out of the box Laminar provides per job `workspaces`, which are common to 
all runs of a particular Laminar job. 

Since I make extensive use of chained jobs to automate the building, 
testing and then potential deploying of a *project*, *per job* 
`workspaces` while nice, become combersome. 

Instead, since Laminar is so cleanly structured, I have implemented 
'project' `workspaces`. 

To do this I have created a `workspaces` directory as a sister directory 
to the existing `archive` and `run` directories. I then create one 
subdirectory for each probject, which I can further subdivide, for 
example, by $DISTribution. I typcially provide a `PWORKSPACE` environment 
variable which an individual job can use to local the `workspace` 
contents for that project. 

My associated `clearLaminarWorkspaces` command, clears out ALL of these 
project workspaces. 
