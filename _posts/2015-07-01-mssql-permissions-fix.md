---
layout: post
title:  "Fixing MSSQL permissions after a database restore"
date:   2015-07-01 10:30:00
categories: configuration
---

This is one of those posts that I know I'll need to come back to sooner or later. After restoring an MSSQL database in a different instance these are just a couple of commands that will usually sort out conflicting security principles and get you back to being productive.

Sometimes fixing permissions can be a bit more involved but in my experience the following works in more than 80% of cases.

{% highlight sql %}
/* Fix permissions when a login exists */
EXEC sp_change_users_login 'Auto_Fix', 'user';

/* Fix permissions by creating a new login that doesn't exist. */
EXEC sp_change_users_login 'Auto_Fix', 'user', 'login', 'password';
{% endhighlight %}

