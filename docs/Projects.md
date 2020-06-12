# Projects and Project workspaces

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
variable which an individual job can use to locate the `workspace` 
contents for that project. 

My associated `clearLaminarWorkspaces` command, clears out ALL of these 
project workspaces. 

Following a [suggestion from a previous 
issue](https://github.com/ohwgiles/laminar/issues/124#issuecomment-636382799),
all jobs for a given project are prepended with a projectName and dash, 
for example: 

```
  pdf2htmlex-build.run
  pdf2htmlex-test-appImage.run
  pdf2htmlex-test-dpgk.run
  pdf2htmlex-test-docker.run
```

Then using the [global before 
script](https://laminar.ohwg.net/docs.html#Script-execution-order) 
we extract the project name as the part of the job name *before* the dash 
and use it to define the PROJECT and PWORKSPACE environment variables. We 
also look for any associated project `*.env` or `*.conf` files and 
`laminarc set` all shell environment variable definitions found in these 
files. So, for example, any shell environment variable definitions found 
in either of the `pdf2htmlex` project files: 

```
  pdf2htmlex.conf
  pdf2htmlex.env
```

are `laminar set` and will be seen by all scripts in that project. 
