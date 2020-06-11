# Laminar scripts

A collection of [Laminar](https://laminar.ohwg.net/) scripts and jobs 

[Laminar](https://github.com/ohwgiles/laminar)
is an ultra simple and ultra light-weight 
[Continuous-Integration](https://en.wikipedia.org/wiki/Continuous_integration) 
/ [Continuous-Delivery](https://en.wikipedia.org/wiki/Continuous_delivery) 
tool which I am using locally to help me develop a number of projects. 

For my needs, I only really use Laminar in a "semi-continuous" mode. When 
ever I have major changes I fire up Laminar and start the overall build 
process for a particular project. To do this, I have written a small 
collection of bash scripts. 

On my development machine, I already have Docker installed so I use docker 
images to control the build environment. So one of the most important 
Laminar jobs I require, is a job to (re)build the basic docker images I 
use. 
