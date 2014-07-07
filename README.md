# Requirement

Local user should be in this group: **docker**

Then fetch this repository:

    git clone git@github.com:TeMPO-Consulting/docker-odoo

And go into this one.

# Build

Then you have to build an image from this repository:

     docker build -t odoo:latest .

Now you have to run the container. But have a look to an example of what could be done:

    docker run -d -p 2280:22 -p 8061 --name odoo odoo:latest /usr/bin/supervisord

We know that:

  * it will be run in daemon (**-d** option)
  * we open port 8061 (Cf. ``docker ps`` to know which port)
  * we open port 22 on port 2280 on our machine so that we use 2280 to access to the docker
  * the name of the container will be **odoo**
  * on our machine, we use the **odoo:latest** docker base to create this container (we built it previously)
  * the default command used when the container is launched is **/usr/bin/supervisord** so that all services will be available (ssh, postgresql, etc.)

And now you're running postgreSQL/ssh for Unifield.

# Access to the container

As SSHD is launched on the machine, we just have to do:

    ssh -p 2280 docker@localhost

And we enter in the container.

# Launch odoo

After having **Access to the container**, just do this:

    cd /opt/odoo
    ./odoo.py

Odoo will be launched.

# Manage containers

If you want to stop the container:

    docker stop odoo

To run an existing container:

    docker start odoo

That's all!

# Find 5432 port

Just do this:

    docker inspect --format='{{(index (index .NetworkSettings.Ports "5432/tcp") 0).HostPort}}' unifield

# Use X11 capabilities and run Eclipse into the Docker environment

**Before** launching the build of this directory, decomment this line:

    RUN apt-get install -y eclipse

Then:

  * build your docker
  * run a docker
  * access it via ssh using this command:

    ssh -X -p 2280 docker@localhost

Then, in the given SSH prompt, just tape:

    eclipse

and it will launch Eclipse.
