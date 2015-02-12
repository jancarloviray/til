# How to attach and then detach without killing the process?

When you press ctrl+c, you are doing a SIGKILL on the process.

By default design, docker quits and stops the container.

To detach without killing the container, run ctrl+p + ctrl+q.

Read more about it here at the official docs: [Running an Interactive Shell](http://docs.docker.io/en/latest/use/basics/#running-an-interactive-shell).
