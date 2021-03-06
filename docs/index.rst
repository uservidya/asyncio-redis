.. asyncio_redis documentation master file, created by
   sphinx-quickstart on Thu Oct 31 08:50:13 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

asyncio_redis
=============
Redis client for PEP 3156. (Tulip)

Features
--------

- Works for the asyncio (PEP3156) event loop
- No dependencies
- Connection pooling
- Automatic conversion from unicode (Python) to bytes (inside Redis.)
- Bytes and str protocols.
- Completely tested
- Blocking calls and transactions supported
- Streaming of some multi bulk replies

Installation
------------

::

    pip install asyncio_redis

Example
-------

::

    import asyncio
    from asyncio_redis import Connection

    @asyncio.coroutine
    def run():
        # Create Redis connection
        connection = yield from Connection.create(port=6379, poolsize=10)

        # Set a key
        yield from connection.set(u'my_key', u'my_value')

        # Get a key
        result = yield from connection.get(u'my_key')
        print(result)

    if __name__ == '__main__':
        asyncio.get_event_loop().run_until_complete(run())

See :ref:`the reference <redis-reference>` to learn more about how to call other
Redis commands.


Transaction example
-------------------

::

    import asyncio
    from asyncio_redis import Connection

    @asyncio.coroutine
    def example(loop):
        # Create Redis connection
        connection = yield from Connection(port=6379, poolsize=10)

        # Create transaction
        transaction = yield from connection.multi()

        # Run commands in transaction (they return future objects)
        f1 = yield from transaction.set('key', 'value')
        f1 = yield from transaction.set('another_key', 'another_value')

        # Commit transaction
        yield from transaction.exec()

        # Retrieve results
        result1 = yield from f1
        result2 = yield from f2


Reference
---------

:ref:`View the reference with all commands <redis-reference>`

View `the source code at GitHub`_

.. _the source code at GitHub: https://github.com/jonathanslenders/asyncio-redis


Author and License
==================

The ``asyncio_redis`` package is written by Jonathan Slenders.
It's BSD licensed and freely available.

Feel free to improve this package and `send a pull request`_.

.. _send a pull request: https://github.com/jonathanslenders/asyncio-redis


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`


.. toctree::
   :maxdepth: 2

   pages/reference
   pages/replies
