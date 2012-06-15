============
RQ Scheduler
============

`RQ Scheduler <https://github.com/ui/rq-scheduler>`_ is a small package that
adds job scheduling capabilities to `RQ <https://github.com/nvie/rq>`_,
a `Redis <http://redis.io/>`_ based Python queuing library.

============
Requirements
============

* `RQ`_

============
Installation
============

You can install `RQ Scheduler`_ via pip::

    pip install rq-scheduler

=====
Usage
=====

Schedule a job involves doing two different things:
1. Putting a job in the scheduler
2. Running a scheduler that will move scheduled jobs into queues when the time comes

----------------
Scheduling a Job
----------------

Here's how to put a job in the scheduler::

    from redis import Redis
    from rq_scheduler import Scheduler
    from datetime import datetime
    
    # Instantiates a scheduler for the "default" queue
    scheduler = Scheduler('default', connection=Redis()) 
    
    # Puts a job into the scheduler. The API is similar
    # to rq except that it takes a datetime object as first argument
    scheduler.enqueue(datetime(2020, 01, 01, 1, 1), func)
    scheduler.enqueue(datetime(2025, 10, 10, 3, 4), func, foo, bar=baz)

---------------------
Running the scheduler
---------------------

`RQ Scheduler`_ comes with a script ``rqscheduler`` that runs a scheduler
process that polls Redis once every minute and move scheduled jobs to the
relevant queues when they need to be executed::

    rqscheduler --host localhost --port 6379 --db 0

The script accepts these arguments:

* ``-H`` or ``--host``: Redis server to connect to
* ``-p`` or ``--port``: port to connect to
* ``-d`` or ``--db``: Redis db to use
* ``-P`` or ``--password``: password to connect to Redis