# Drupal Global Redirect

This is a fork of http://drupal.org/project/globalredirect in response to my comments on http://drupal.org/node/1220560. The purpose is to provide a sensible way to manage multiple URL Aliases for the same system path. Currently, the path methods just return the "most recent" URL alias, which is to say the alias with the highest primary key.

Since there's not a pretty way to allow us to set the order, we'll do it an ugly way: by overwriting each alternate URL alias so that their pids (the primary key on the url_alias table) are in the same order as their weight. Time for you to exclaim, "What!?!" Yeah - I said it was ugly.

For example:

If we start with aliases for "node/1" like:

* 1: super/node
* 2: awesome/node

and we set the weights for these aliases to put "super/node" first (remember, Drupal is always picking the path with the highest pid), we'll end up with 

* 1: awesome/node
* 2: super/node

One way of looking at it is to say that we're renaming the existing aliases in order of their weights, so that the system paths "work" in the way that we want them to. It doesn't seem right, and I don't like it, but it does seem effective. In the first case, the path the system uses for "node/1" is "awesome/node", and the second it's "super/node". Because Global Redirect uses the alias Drupal prefers, that means our redirects go to the right canonical URL.

It's not pretty, but it seems to get the job done.