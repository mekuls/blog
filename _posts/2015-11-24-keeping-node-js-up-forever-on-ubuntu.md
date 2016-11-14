---
layout: post
title:  "Keeping NodeJS up forever on Ubuntu"
date:   2015-11-24 09:00:00
categories: nodejs javascript 
---

Here's a simple program written in nodeJS

<script src="https://gist.github.com/lukemuccillo/6b89df3cfec327d1d0dc.js"></script>

The objective of this post is to document how we can configure init.d and forever to serve this application as if it were a daemon.

##Install: 

{% highlight bash %}
sudo apt-get update
sudo apt-get install -y nodejs
sudo apt-get install -y npm
sudo npm install forever
{% endhighlight %}
 
Here I create a symlink to nodejs because the install doesn't add the binary to my PATH.

{% highlight bash %}
sudo ln -s "$(which nodejs)" /usr/bin/node
{% endhighlight %}

Add a startup definition:

/etc/init.d/udpqos

{% highlight bash %}
#!/bin/bash
NAME="udpqos"
NODE_BIN_DIR=/usr/local/lib/node_modules/forever/bin
NODE_PATH=/usr/bin/nodejs
APPLICATION_DIRECTORY=/opt/scripts/udpqos
APPLICATION_START=udpqos.js
PIDFILE=/var/run/$NAME.pid
LOGFILE=/var/log/$NAME.log
PATH=$NODE_BIN_DIR:$PATH
export NODE_PATH=$NODE_PATH
export PATH=$PATH

 
start() {
        echo "Starting $NAME"
        $NODE_BIN_DIR/forever --pidFile $PIDFILE --sourceDir $APPLICATION_DIRECTORY \
                -a -l $LOGFILE --minUptime 5000 --spinSleepTime 2000 \
                start $APPLICATION_START &
        RETVAL=$?
}
stop() {
        if [ -f $PIDFILE ]; then
                echo "Shutting down $NAME"
                rm -f $PIDFILE
                RETVAL=$?
        else
                echo "$NAME is not running."
                RETVAL=0
        fi
}
restart() {
        echo "Restarting $NAME"
        stop
        start
}
status() {
        echo "Status for $NAME:"
        $NODE_BIN_DIR/forever list
        RETVAL=$?
}
case "$1" in
        start)
                start
                ;;
        stop)
                stop
                ;;
        status)
                status
                ;;
        restart)
                restart
                ;;
        *)
                echo "Usage: {start|stop|status|restart}"
                exit 1
                ;;
esac
exit $RETVAL
{% endhighlight %}

With the above in place we make it executable and configure to run at startup

{% highlight bash %}
chmod a+x /etc/init.d/udpqos 
update-rc.d udpqos defaults
/etc/init.d/udpqos start
{% endhighlight %}