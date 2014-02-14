.. _hooks:

=====
Hooks
=====

Hooks enable you to execute custom code at certain points during a Blockwart run. This is useful for integrating with other systems e.g. for team notifications, logging or statistics.

To use hooks, you need to create a subdirectory in your repo called ``hooks``. In that directory you can place an arbitrary number of Python source files. If those source files define certain functions, these functions will be called at the appropriate time.


Example
-------

``hooks/my_awesome_notification.py``:

.. code-block:: python

    from my_awesome_notification_system import post_message

    def node_apply_start(repo, node, interactive=False, **kwargs):
        post_message("Starting apply on {}, everything is gonna be OK!".format(node.name))



.. note::

	Always define your hooks with ``**kwargs`` so we can pass in more information in future updates without breaking your hook.


Functions
---------

This is a list of all functions a hook file may implement.


.. py:function:: apply_start(repo, target, nodes, interactive=False, **kwargs)

    Called when you start a ``bw apply`` command.

    :param Repository repo: The current repository (instance of :py:class:`blockwart.repo.Repository`).
    :param str target: The group or node name you gave on the command line.
    :param list nodes: A list of node objects affected.
    :param bool interactive: Indicates whether the apply is interactive or not.


.. py:function:: apply_end(repo, target, nodes, duration=None, **kwargs)

    Called when a ``bw apply`` command completes.

    :param Repository repo: The current repository.
    :param str target: The group or node name you gave on the command line.
    :param list nodes: A list of node objects affected.
    :param timedelta duration: How long the apply took.


.. py:function:: item_apply_start(repo, node, item, **kwargs)

    Called each time a ``bw apply`` command reaches a new item.

    :param Repository repo: The current repository.
    :param Node node: The current node.
    :param Item item: The current item.


.. py:function:: item_apply_end(repo, node, item, duration=None, status_before=None, status_after=None, **kwargs)

    Called each time a ``bw apply`` command completes processing an item.

    :param Repository repo: The current repository.
    :param Node node: The current node.
    :param Item item: The current item.
    :param timedelta duration: How long the apply took.
    :param ItemStatus status_before: An object with these attributes: ``aborted``, ``correct``, ``fixable``, ``info``.
    :param ItemStatus status_after: See ``status_before``.


.. py:function:: node_apply_start(repo, node, **kwargs)

    Called each time a ``bw apply`` command reaches a new node.

    :param Repository repo: The current repository.
    :param Node node: The current node.


.. py:function:: node_apply_end(repo, node, duration=None, result=None, **kwargs)

    Called each time a ``bw apply`` command finishes processing a node.

    :param Repository repo: The current repository.
    :param Node node: The current node.
    :param timedelta duration: How long the apply took.
    :param ApplyResult result: An object with these attributes: ``correct``, ``fixed``, ``aborted``, ``unfixable``, ``failed``.


.. py:function:: node_run_start(repo, node, command, **kwargs)

    Called each time a ``bw run`` command reaches a new node.

    :param Repository repo: The current repository.
    :param Node node: The current node.
    :param str command: The command that will be run on the node.


.. py:function:: node_run_start(repo, node, command, duration=None, return_code=None, stdout="", stderr="", **kwargs)

    Called each time a ``bw run`` command finishes on a node.

    :param Repository repo: The current repository.
    :param Node node: The current node.
    :param str command: The command that was run on the node.
    :param timedelta duration: How long it took to run the command.
    :param int return_code: Return code of the remote command.
    :param str stdout: The captured stdout stream of the remote command.
    :param str stderr: The captured stderr stream of the remote command.


.. py:function:: run_start(repo, target, nodes, command, **kwargs)

    Called each time a ``bw run`` command starts.

    :param Repository repo: The current repository.
    :param str target: The group or node name you gave on the command line.
    :param list nodes: A list of node objects affected.
    :param str command: The command that will be run on the node.


.. py:function:: run_start(repo, target, nodes, command, duration=None, **kwargs)

    Called each time a ``bw run`` command finishes.

    :param Repository repo: The current repository.
    :param str target: The group or node name you gave on the command line.
    :param list nodes: A list of node objects affected.
    :param str command: The command that was run.
    :param timedelta duration: How long it took to run the command on all nodes.