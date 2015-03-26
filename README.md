# revolver

Revolver is a simple utility that uses ansible to distribute Fernet keys to
remote Keystone servers. When deploying multiple Keystone servers with Fernet
tokens, every node needs the same keys in order to validate tokens generated a
different Keystone server.

This utility is designed to be a simple way to ensure each Keystone server has
the same key repository, which is handy for testing multi-Keystone deployments
deployed with
[keystone-deploy](https://github.com/dolph/keystone-deploy/tree/fernet-tokens>).
This utility doesn't claim to be production ready, it is a tool to aid in
testing setup.

The current playbook, which distributes the key repository, assumes that it is
being run from a Keystone server and it will copy the local Keystone server's
key repository to all other Keystone servers listed in the `inventory` file.

For example, if you have three Keystone servers in your deployment:
  * keystonealpha.example.com
  * keystonebeta.example.com
  * keystonegamma.example.com

You could run revolver from keystonealpha.example.com to push it's key
repository to the other Keystone nodes with the following inventory file:

``
$ cat inventory
keystonebeta.example.com
keystonegamma.example.com
$ ansible-playbook -i inventory --sudo deploy.yaml
``

Note that this will require the public ssh key from keystonealpha.example.com
to be copied over to keystonebeta.example.com and keystonegamma.example.com.

This playbook should be run after every subsequent key rotation operation on
keystonealpha.example.com.
