# Laminar control scripts

This directory captures the command line scripts used to control my 
semi-continuous use of Laminar. 

- `startLaminar` starts laminar running, echos the UI url, Laminar 
  documentation url and runs in the foreground (waiting for Ctrl-C to 
  stop). 

- `clearLaminar` clears out laminar's database, archive and run 
  directories. (Effectively cleaning all of laminar's saved state). 

  The configuration directory, `laminar/tasks/cfg` is untouched.

- `clearLaminarWorkspaces` clears out my laminar's workspaces 
  directory. (See the Worksapces.md in the document directory).

  The configuration directory, `laminar/tasks/cfg` is untouched.
