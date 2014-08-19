## Cory's Notes

This stuff is all hacked together from what I've read below, alongside [this dotcloud tutorial](http://docs.dotcloud.com/tutorials/python/mobwrite/). I'm treating most of it as black magic.

There are two big components, the Gateway and the Daemon (man that sounds dark and gothic).

### Gateway

This is just an incredibly simple python server that pushes any requests over to the local daemon (via socket). The tutorial covers setting it up as [WSGI](http://docs.dotcloud.com/tutorials/python/mobwrite/#wsgi-py-file).

### Daemon

This listens through sockets for any incoming data from the gateway, does some crazy [Operational Transform](http://en.wikipedia.org/wiki/Operational_transformation) magic and then spits back out the latest copy of the data back to whoever asks for it. It doesn't have any external dependencies.

Per the tutorial, I set it up through supervisord and then just pray a lot.

### Connecting

[think.cs.vt.edu/mobwrite/q/](Here) is where that ends up being available through the Javascript libraries. Not much to look at from a browser, there's still a good bit of client-side magic to make everything happen.

## Okey-dokey

Basically this is an SVN import of the [google-mobwite project over at google code](http://code.google.com/p/google-mobwrite) with some minor alterations
and patching to the gateway script up and running via WSGI instead of mod_python. I have also cleaned away all the other stuff that I didn't need (java, php, etc).

The main meat of the updates are to be found in `daemon/gateway.py`, which is pretty much a straight up port of Neil Fraser's original
python gateway to run under WSGI instead of mod_python. Verrra nice.

### Cajun style

I'm just going to assume you are using virtualenv & virtualevwrapper because you should be...

1. `virtualenv --no-site-packages mobwrite`
2. `cdvirtualenv`
3. `git clone git://github.com/plasticine/google-mobwrite.git`

Open a couple of new terminals (& move into the new virtualenv again, etc) you can;

1. First terminal;
    * `cd google-mobwrite/daemon`
    * `python mobwrite_daemon.py`
2. Second terminal;
    * `cd google-mobwrite/daemon`
    * `python gateway.py`

Now you should be able to test everything is working locally over yonder: [http://localhost:8000/?editor](http://localhost:8000/?editor).
