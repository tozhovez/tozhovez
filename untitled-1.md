# Advanced Python Scheduler — APScheduler 3.6.3.post13 documentation

[![Build Status](https://travis-ci.com/agronholm/apscheduler.svg?branch=master)](https://travis-ci.com/agronholm/apscheduler) [![Code Coverage](https://coveralls.io/repos/github/agronholm/apscheduler/badge.svg?branch=master)](https://coveralls.io/github/agronholm/apscheduler?branch=master)

Advanced Python Scheduler \(APScheduler\) is a Python library that lets you schedule your Python code to be executed later, either just once or periodically. You can add new jobs or remove old ones on the fly as you please. If you store your jobs in a database, they will also survive scheduler restarts and maintain their state. When the scheduler is restarted, it will then run all the jobs it should have run while it was offline [1]().

Among other things, APScheduler can be used as a cross-platform, application specific replacement to platform specific schedulers, such as the cron daemon or the Windows task scheduler. Please note, however, that APScheduler is **not** a daemon or service itself, nor does it come with any command line tools. It is primarily meant to be run inside existing applications. That said, APScheduler does provide some building blocks for you to build a scheduler service or to run a dedicated scheduler process.

APScheduler has three built-in scheduling systems you can use:

* Cron-style scheduling \(with optional start/end times\)
* Interval-based execution \(runs jobs on even intervals, with optional start/end times\)
* One-off delayed execution \(runs jobs once, on a set date/time\)

You can mix and match scheduling systems and the backends where the jobs are stored any way you like. Supported backends for storing jobs include:

* Memory
* [SQLAlchemy](http://www.sqlalchemy.org/) \(any RDBMS supported by SQLAlchemy works\)
* [MongoDB](http://www.mongodb.org/)
* [Redis](http://redis.io/)
* [RethinkDB](https://www.rethinkdb.com/)
* [ZooKeeper](https://zookeeper.apache.org/)

APScheduler also integrates with several common Python frameworks, like:

* [asyncio](http://docs.python.org/3.4/library/asyncio.html) \([**PEP 3156**](https://www.python.org/dev/peps/pep-3156)\)
* [gevent](http://www.gevent.org/)
* [Tornado](http://www.tornadoweb.org/)
* [Twisted](http://twistedmatrix.com/)
* [Qt](http://qt-project.org/) \(using either [PyQt](http://www.riverbankcomputing.com/software/pyqt/intro) , [PySide2](https://wiki.qt.io/Qt_for_Python)\)\_ or [PySide](http://qt-project.org/wiki/PySide)\)

