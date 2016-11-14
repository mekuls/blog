#Just a collection of commands 


**The Guts**

{%highlight%}
docker run -it --rm ubuntu:14.04 /bin/bash
{%endhighlight%}


**Remove all untagged images**
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
