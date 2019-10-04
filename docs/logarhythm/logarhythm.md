Module logarhythm.logarhythm
============================

Functions
---------

    
`breakpoint(condition=True)`
:   Same as getLogger().breakpoint(condition)

    
`build_format(time='hms', logger_name=True, process_name=False, thread_name=False, level=True)`
:   This function tries to simplify generating logger formatting expressions, by determining the style and order.
    The user just needs to specify what information they want to include.
    
    Input valid values (first option listed is the default):
    
    - time = 'hms' | 'full' | 'elapsed_msec' | None
    
        - 'hms' = HH:MM:SS i.e. hour:minute:second within the current day
    
        - 'full' = yyyy-mm-dd/HH:MM:SS i.e. date + hour:minute:second
    
        - 'elapsed_msec' = Number of milliseconds elapsed since logging module was loaded
        
        - None =  Do not include time
    
    - logger_name = True | False
    
        - Whether to include the logger name
    - process_name = False | True
    
        - Whether to include the process name (for multiprocessing)
    - thread_name = False | True
    
        - Whether to include the thread name (for multithreading)
    
    - level = True | False
    
        - Whether to include the level of the message
    
    >>> import logarhythm
    >>> logger = logarhythm.getLogger()
    >>> logarhythm.build_format(time=None) == (u'[%(name)s :%(levelname)s] %(message)s', u'%H:%M:%S', u'%')
    True
    
    >>> build_format('full',False,True,True,False) == ('[%(asctime)s Process="%(processName)s" Thread="%(threadName)s"] %(message)s', '%Y-%m-%d/%H:%M:%S', '%')
    True
    >>> build_format('elapsed_msec') == ('[%(relativeCreated)s %(name)s :%(levelname)s] %(message)s', '%H:%M:%S', '%')
    True

    
`capture_debug_callback(logger, caller_frame_info, match, level)`
:   This is the default callback for the Logger.capture() method.
    This results in debug mode being entered when a logged message matches the captured pattern.

    
`critical(msg, *args, **kwargs)`
:   Logs a message on the default logger (default = logger for \_\_name\_\_)
    
    See logging module for *args and **kwargs documentation.
    
    >>> logger = getLogger()
    >>> with logger.use(doctest_mode=True,level=DEBUG):
    ...    critical('test') #uses same object named logger above, even though this is a module level function and not a Logger method
    ... 
    [logarhythm.logarhythm :CRITICAL] test

    
`debug(msg, *args, **kwargs)`
:   Logs a message on the default logger (default = logger for \_\_name\_\_)
    
    See logging module for *args and **kwargs documentation.
    >>> logger = getLogger()
    >>> with logger.use(doctest_mode=True,level=DEBUG):
    ...    debug('test') #uses same object named logger above, even though this is a module level function and not a Logger method
    ... 
    [logarhythm.logarhythm :DEBUG] test
    >>> logger.level
    0

    
`error(msg, *args, **kwargs)`
:   Logs a message on the default logger (default = logger for \_\_name\_\_)
    
    See logging module for *args and **kwargs documentation.
    
    >>> logger = getLogger()
    >>> with logger.use(doctest_mode=True,level=DEBUG):
    ...    error('test') #uses same object named logger above, even though this is a module level function and not a Logger method
    ... 
    [logarhythm.logarhythm :ERROR] test

    
`exception(msg, *args, **kwargs)`
:   Logs a message on the default logger (default = logger for \_\_name\_\_)
    
    See logging module for *args and **kwargs documentation.
    
    >>> logger = getLogger()
    >>> with logger.use(doctest_mode=True,level=DEBUG):
    ...    try:
    ...       x = []
    ...       print(x[0])
    ...    except IndexError:
    ...       exception('test') #uses same object named logger above, even though this is a module level function and not a Logger method
    ... 
    [logarhythm.logarhythm :ERROR] test
    Traceback (most recent call last):
      File "<doctest logarhythm.logarhythm.exception[1]>", line 4, in <module>
        print(x[0])
    IndexError: list index out of range

    
`getLogger(name=None)`
:   Returns the logarhythm logger with the given name
    
    >>> Logger() is getLogger()
    True
    >>> getLogger() is getLogger(__name__)
    True
    >>> Logger() is getLogger('.')
    True
    >>> Logger('.') is getLogger('.')
    True
    >>> Logger('.').child('doctest') is getLogger('.doctest')
    True

    
`getLogger_logging_module(name=None)`
:   Returns a Logger as defined in the logging module but default name is \_\_name\_\_ instead of root

    
`info(msg, *args, **kwargs)`
:   Logs a message on the default logger (default = logger for \_\_name\_\_)
    
    >>> logger = getLogger()
    >>> with logger.use(doctest_mode=True,level=DEBUG):
    ...    info('test') #uses same object named logger above, even though this is a module level function and not a Logger method
    ... 
    [logarhythm.logarhythm :INFO] test

    
`log(level, msg, *args, **kwargs)`
:   Logs a message on the default logger (default = logger for \_\_name\_\_)
    
    See logging module for *args and **kwargs documentation.
    
    >>> logger = getLogger()
    >>> with logger.use(doctest_mode=True,level=DEBUG):
    ...    log(DEBUG,'test') #uses same object named logger above, even though this is a module level function and not a Logger method
    ... 
    [logarhythm.logarhythm :DEBUG] test

    
`monitor_attr_debug_callback(logger, caller_frame_info, target, attr_name, old_value, new_value)`
:   This is an optional callback that can be used with the Logger.monitor_attr() method.
    If utilized, then changes to the monitored attribute will result in debug mode being entered.

    
`monitor_call_debug_callback(logger, caller_frame_info, target_callable, args, kwargs)`
:   This is an optional callback that can be used with the Logger.monitor_call() method.
    If utilized, then calling the monitored method will result in debug mode being entered.

    
`parse_tlm(s, tlm_channel=None)`
:   This function searches a string for telemetry log messages created by the logarhythm.tlm() function.
    All logged telemetry messages will be yielded as dictionaries mapping the original keyword arguments to their values.

    
`set_auto_debug(value)`
:   

    
`set_disarm_logging_module(value)`
:   Monkey-patches the logging module to make the default behavior for module level functions to be to grab the module-level logger instead of the root logger.
    
    This helps to prevent third party modules from broadcasting logging methods on the root logger by following the official basic logging instructions.
    
    Afffects the following functions:
    - debug
    - info
    - warning
    - error
    - critical
    - exception
    - log
    - getLogger
    
    This function is automatically called with disarm=True when logarhythm is imported.
    
    To restore the logging module to normal, call this function with disarm=False
    
    >>> GlobalSettings.set_disarm_logging_module(False) #restores the original functions to the logging module
    >>> GlobalSettings.set_disarm_logging_module(True) #replaces the logging module functions with the logarhythm equivalents

    
`set_end_interactive(value)`
:   end_interactive = When True, enter interactive mode when script is done processing
    
    >>> GlobalSettings.set_end_interactive(True) #will cause interpreter to go to interactive mode when script stops running
    >>> GlobalSettings.set_end_interactive(False) #interpreter will terminate when script stops running

    
`short_repr(x)`
:   This is the same as repr() except it limits the string to 35 characters.
    If the string is more than 35 characters long, this will print the first 16 and the last 16 characters separated by an ellipsis "...".

    
`tlm(tlm_channel=None, **kwargs)`
:   This function produces a log message string to be passed into a logger method that captures parameter names and values based on the provided keyword arguments.
    The keyword arguments and values are encoded into JSON messages. Only values that can be encoded in JSON are allowed.
    Logfiles that contain messages generated from this function can be parsed using the parse_tlm() function to extract these telemetry records.
    
    The first parameter, tlm_channel is a special optional parameter that will be attached to the logged message.
    The parse_tlm function can be configured to only include messages matching a specified tlm_channel value.
    
    >>> logger = Logger('doctest')
    >>> with logger.use(stdout=False,doctest_mode=True,level=INFO):
    ...    with logger.stream_open() as sh:
    ...       logger.info(tlm('channel A',x=5,y=10))
    ...       logger.info(tlm('channel A',x=9,y=11))
    ...       logger.info(tlm(z='trying to be tricky with )00000000_tlm_end inside the data itself'))
    ... 
    ... 
    >>> list(parse_tlm(sh.getvalue())) == [{'x':5,'y':10},{'x':9,'y':11},{'z':'trying to be tricky with )00000000_tlm_end inside the data itself'}]
    True
    >>> list(parse_tlm(sh.getvalue(),'channel A')) == [{'x':5,'y':10},{'x':9,'y':11}]
    True

    
`warning(msg, *args, **kwargs)`
:   Logs a message on the default logger (default = logger for __name__)
    
    See logging module for *args and **kwargs documentation.
    
    >>> logger = getLogger()
    >>> with logger.use(doctest_mode=True,level=DEBUG):
    ...    warning('test') #uses same object named logger above, even though this is a module level function and not a Logger method
    ... 
    [logarhythm.logarhythm :WARNING] test

Classes
-------

`FileHandle(logger, path, mode='a', encoding=None, delay=None)`
:   A handle that represents a file object which has logging messages written to it

    ### Ancestors (in MRO)

    * logarhythm.logarhythm.LoggingHandle

    ### Methods

    `close(self)`
    :   When closed, the logger will no longer write to the file and the file itself will be closed.

`GlobalSettings(*args, **kwargs)`
:   Manages global level settings

    ### Class variables

    `auto_debug_enabled`
    :   Whether or not unhandled exceptions will result in entering debug mode

    `disarm_logging_module`
    :   Whether or not the logging module functions are changed to use module level loggers instead of the root logger

    `end_interactive_enabled`
    :   Whether or not the script will go into interactive mode when it finishes executing

    ### Static methods

    `set_auto_debug(value)`
    :

    `set_disarm_logging_module(value)`
    :   Monkey-patches the logging module to make the default behavior for module level functions to be to grab the module-level logger instead of the root logger.
        
        This helps to prevent third party modules from broadcasting logging methods on the root logger by following the official basic logging instructions.
        
        Afffects the following functions:
        - debug
        - info
        - warning
        - error
        - critical
        - exception
        - log
        - getLogger
        
        This function is automatically called with disarm=True when logarhythm is imported.
        
        To restore the logging module to normal, call this function with disarm=False
        
        >>> GlobalSettings.set_disarm_logging_module(False) #restores the original functions to the logging module
        >>> GlobalSettings.set_disarm_logging_module(True) #replaces the logging module functions with the logarhythm equivalents

    `set_end_interactive(value)`
    :   end_interactive = When True, enter interactive mode when script is done processing
        
        >>> GlobalSettings.set_end_interactive(True) #will cause interpreter to go to interactive mode when script stops running
        >>> GlobalSettings.set_end_interactive(False) #interpreter will terminate when script stops running

`LogarhythmException(*args, **kwargs)`
:   Base class for logarhythm specific Exceptions

    ### Ancestors (in MRO)

    * builtins.Exception
    * builtins.BaseException

`Logger(name=None, **kwargs)`
:   Primary class for logarhythm. All logarhythm functionalities can be accessed from this class.
    
    When Logger objects are created, they will inherit the following attributes from their parent logger:
        stderr
        stdout
        format_fmt
        format_time_fmt
        format_style
    
    The root logger has no parent, and will have the following settings by default:
        stderr = True
        stdout = False

    ### Class variables

    `CRITICAL`
    :   CRITICAL = 50

    `DEBUG`
    :   DEBUG = 10

    `ERROR`
    :   ERROR = 40

    `INFO`
    :   INFO = 20

    `NOTSET`
    :   NOTSET = 0 - Implies a logger uses its parents level

    `OFF`
    :   OFF = 60

    `WARNING`
    :   WARNING = 30

    `root_logger`
    :   Primary class for logarhythm. All logarhythm functionalities can be accessed from this class.
        
        When Logger objects are created, they will inherit the following attributes from their parent logger:
            stderr
            stdout
            format_fmt
            format_time_fmt
            format_style
        
        The root logger has no parent, and will have the following settings by default:
            stderr = True
            stdout = False

    ### Static methods

    `get_loggers()`
    :   Returns a dictionary mapping all existing logger names to logger objects.
        >>> logger = getLogger()
        >>> logger.name in Logger.get_loggers()
        True
        >>> root_logger.name in Logger.get_loggers()
        True

    ### Instance variables

    `auto_debug`
    :   Tied to the global flag for auto_debug. 
        
        When True, unhandled exceptions will result in entering debug mode.

    `dev_mode`
    :   This is a shortcut attribute.
        It always evaluates to False.
        When set to True, it is a shortcut for:
            .stderr = True
            .stdout = False
            .auto_debug = True
            .level = DEBUG
            .captures_disabled=False
            .profiling_disabled=False
            .monitoring_disabled = False
            .debugging_disabled = False
        
        >>> logger = getLogger('doctest')
        >>> with logger.use(dev_mode=True,stderr=False,stdout=True,format=build_format(time=None)):
        ...  logger.debug('debug')
        [doctest :DEBUG] debug
        >>> logger = getLogger('doctest')
        >>> with logger.use(dev_mode=True):
        ...  logger.dev_mode == False
        ... 
        True

    `disarm_logging_module`
    :   Tied to the global flag for end_interactive.
        
        When True, the logging module will have its top level functions changed to use a module level logger by default instead of the root logger.

    `doctest_mode`
    :   This is a shortcut attribute.
        It always evaluates to False.
        When set to True, it is a shortcut for:
            .stderr = False
            .stdout = True
            .format = build_format(time=None)
            .debugging_disabled = True
        >>> logger = getLogger('doctest')
        >>> with logger.use(doctest_mode=True):
        ...  logger.doctest_mode == False
        ... 
        True

    `end_interactive`
    :   Tied to the global flag for end_interactive.
        
        When True, the script will end in interactive mode.

    `format`
    :

    `level`
    :   Will log messages with levels at or higher than the logger's level

    `name`
    :   The logger name

    `prod_mode`
    :   This is a shortcut attribute.
        It always evaluates to False.
        When set to True, it is a shortcut for:
            .stderr = True
            .stdout = False
            .auto_debug = False
            .end_interactive_mode = False
            .level = NOTSET
            .captures_disabled = True
            .profiling_disabled = True
            .monitoring_disabled = True
            .debugging_disabled = True
        >>> from time import sleep
        >>> logger = getLogger('doctest')
        >>> with logger.use(prod_mode=True,level=INFO):
        ...  print(logger.prod_mode)
        ...  with logger.profile():
        ...   sleep(0.1) 
        ... 
        False

    `stderr`
    :   When set to True, messages will be logged to stderr.

    `stdout`
    :   When set to True, messages will be logged to stdout.

    ### Methods

    `breakpoint(self, condition=True, caller_frame_info=None)`
    :   Stops in debug mode when the provided condition is met.
        
        >>> logger = Logger('doctest') 
        >>> with logger.use(doctest_mode=True):
        ...    for x in range(100):
        ...       logger.breakpoint(x == 35)
        ... 
        [doctest :WARNING] Breakpoint triggered for Logger at <doctest logarhythm.logarhythm.Logger.breakpoint[1]> line 3

    `capture(self, pattern, flags=0, target_level=None, callback=<function capture_debug_callback>)`
    :   Registers a capture pattern to stop in debug mode when a logging message at the appropriate level matches the provided regular expression.
        Can also execute a custom callback to do some other action.
        
        >>> logger = Logger('doctest') 
        >>> logger.capture('is 3')
        >>> logger.capture('some second pattern')
        >>> with logger.use(doctest_mode=True,level=DEBUG):
        ...    for x in range(5):
        ...       logger.debug('x is %d',x)
        ...    logger.clear_captures()
        ...    logger.debug('x is 3')
        ... 
        [doctest :DEBUG] x is 0
        [doctest :DEBUG] x is 1
        [doctest :DEBUG] x is 2
        [doctest :DEBUG] x is 3
        [doctest :WARNING] ALL capture pattern triggered for 'is 3' at <doctest logarhythm.logarhythm.Logger.capture[3]> line 3
        [doctest :DEBUG] x is 4
        [doctest :DEBUG] x is 3

    `child(self, child_name, *args, **kwargs)`
    :   Returns a child logger given a relative name for the child.
        
        Positional and keyword arguments are passed into the Logger constructor.
        
        >>> logger = Logger('doctest')
        >>> logger2 = root_logger.child('doctest')
        >>> logger is logger2 # shows that named loggers are children of the root logger
        True
        >>> subsys1_logger = logger.child('subsys1')
        >>> unit1_logger = subsys1_logger.child('unit1')
        >>> unit2_logger = subsys1_logger.child('unit2')
        >>> print(unit1_logger.name)
        doctest.subsys1.unit1
        >>> with subsys1_logger.use(level = DEBUG):
        ...   with unit2_logger.use(doctest_mode=True):
        ...     unit2_logger.debug("this shows because parent's level = DEBUG")
        ...   with unit1_logger.use(doctest_mode=True):
        ...     unit1_logger.debug('same here')
        ... 
        [doctest.subsys1.unit2 :DEBUG] this shows because parent's level = DEBUG
        [doctest.subsys1.unit1 :DEBUG] same here
        >>> unit1_logger.debug('this does not show because the parents level was set back to NOTSET')
        >>> with unit1_logger.use(doctest_mode=True,level = INFO):
        ...  unit1_logger.info('this does show')
        ... 
        [doctest.subsys1.unit1 :INFO] this does show
        >>> lower = unit1_logger.child('some.deep.logger')
        >>> print(lower.parent.name)
        doctest.subsys1.unit1.some.deep

    `clear_captures(self)`
    :   Clears all capture patterns that were registered for this logger

    `critical(self, msg, *args, **kwargs)`
    :   Logs a message at level CRITICAL. See logging documentation for args and kwargs information.

    `debug(self, msg, *args, **kwargs)`
    :   Logs a message at level DEBUG. See logging documentation for args and kwargs information.

    `error(self, msg, *args, **kwargs)`
    :   Logs a message at level ERROR. See logging documentation for args and kwargs information.

    `exception(self, msg, *args, **kwargs)`
    :   Logs a message at level ERROR. See logging documentation for args and kwargs information.

    `file_open(self, path, mode='a', encoding=None, delay=False)`
    :   Opens a file and starts logging messages to it.
        Returns a FileHandle object.
        
        >>> logger = Logger('doctest')
        >>> with logger.use(stdout=False,doctest_mode=True):
        ...    with logger.file_open('test.log','w') as fh:
        ...       logger.error('this goes to the file')
        ...    
        >>> with open('test.log','r') as f:
        ...    print(f.read().strip())
        ... 
        [doctest :ERROR] this goes to the file
        >>> os.remove('test.log')

    `handler_add(self, handler, use_logger_formatter=True)`
    :   Add a handler as defined in the logging module.
        
        >>> sio = StringIO()
        >>> s = logging.StreamHandler(sio)
        >>> fh = logging.FileHandler('test_handler_add.log',mode='w')
        >>> logger = Logger('doctest')
        >>> with logger.use(level=DEBUG,stdout=False,doctest_mode=True):
        ...    sh = logger.handler_add(s)
        ...    sh2 = logger.handler_add(fh,use_logger_formatter=False)
        ...    logger.debug('test')
        ... 
        >>> sh.close()
        >>> sh2.close()
        >>> print(sio.getvalue().strip())
        [doctest :DEBUG] test
        >>> with open('test_handler_add.log','r') as f:
        ...    print(f.read().strip())
        ... 
        test
        >>> os.remove('test_handler_add.log')

    `info(self, msg, *args, **kwargs)`
    :   Logs a message at level INFO. See logging documentation for args and kwargs information.

    `log(self, level, msg, *args, **kwargs)`
    :   Logs a message at the specified level. See logging documentation for args and kwargs information.

    `monitor_attr(self, target, attr_name=None, label=None, level=10, callback=None)`
    :   Monitors an object for attribute changes.
        If attr_name is specified, only that attribute will be monitored.
        If attr_name is unspecified, all attributes that do not begin with "_" will be monitored for that object.
        
        When a change happens to a monitored attribute, a log message at the specified level will be generated.
        If a label was specified, this label will be included in the log message.
        If a callback was specified, this callback will be called.
        The callback arguments match the monitor_attr_debug_callback() function - see that function for more details.
        
        To remove monitoring on an object, use the unmonitor() method.
        
        >>> logger = getLogger('doctest')
        >>> class Bunch(object): pass
        ... 
        >>> b = Bunch()
        >>> b.x = 1
        >>> b.y = 'this is a quite long string that will be summarized in some way that you will see below'
        >>> with logger.use(doctest_mode=True,level=DEBUG):
        ...  logger.monitor_attr(b,'x')
        ...  logger.monitor_attr(b,'y',label='watching y',callback=monitor_attr_debug_callback)
        ...  b.x = 10
        ...  b.y += ' after this variable changes'
        ...  logger.unmonitor(b)
        ...  b.x = 900
        ... 
        [doctest :DEBUG] Monitored attribute set .x from 1 to 10
        [doctest :DEBUG] Monitored attribute set .y from 'this is a quite... will see below' to 'this is a quite...ariable changes' (label="watching y")

    `monitor_call(self, target_callable, label=None, level=10, callback=None)`
    :   Monitors an callable object for calls.
        
        When the monitored object is called, a log message at the specified level will be generated.
        If a label was specified, this label will be included in the log message.
        If a callback was specified, this callback will be called.
        The callback arguments match the monitor_call_debug_callback() function - see that function for more details.
        
        To remove monitoring on an object, use the unmonitor() method.
        
        >>> logger = getLogger('doctest')
        >>> logger2 = getLogger('doctest2')
        >>> def f(x):
        ...  return x**2
        ... 
        >>> class A(object):
        ...  def __call__(self,x):
        ...   return x**3
        ...  @classmethod
        ...  def g(klass,x):
        ...   return 5*x
        ...  @staticmethod
        ...  def h(x):
        ...   return 30-x
        ... 
        
        >>> a = A()
        >>> with logger.use(doctest_mode=True,level=DEBUG):
        ...  with logger2.use(doctest_mode=True,level=DEBUG):
        ...   logger.monitor_call(f)
        ...   logger2.monitor_call(f) 
        ...   f(5)
        ...   logger.unmonitor(f)
        ...   f(6)
        ...   logger2.unmonitor(f)
        ...   logger.monitor_call(f,label='watching f')
        ...   f(7)
        ...   logger.monitor_call(a,callback=monitor_call_debug_callback)
        ...   logger2.monitor_call(a)
        ...   a(3)
        ...   logger.monitor_call(a.g)
        ...   logger2.monitor_call(a.g)
        ...   a.g(3)
        ...   logger.monitor_call(a.h)
        ...   logger2.monitor_call(a.h)
        ...   a.h(10)
        ...   logger.unmonitor(a)
        ...   logger.unmonitor(a.g)
        ...   logger.unmonitor(a.h)
        ...   logger2.unmonitor(a)
        ...   logger2.unmonitor(a.g)
        ...   logger2.unmonitor(a.h)
        ...   a(3)
        ...   a.g(3)
        ...   a.h(10)
        ... 
        [doctest :DEBUG] Monitored callable called f()
        [doctest2 :DEBUG] Monitored callable called f()
        25
        [doctest2 :DEBUG] Monitored callable called f()
        36
        [doctest :DEBUG] Monitored callable called f() (label="watching f")
        49
        [doctest :DEBUG] Monitored callable called A.__call__()
        [doctest2 :DEBUG] Monitored callable called A.__call__()
        27
        [doctest :DEBUG] Monitored callable called A.g()
        [doctest2 :DEBUG] Monitored callable called A.g()
        15
        [doctest :DEBUG] Monitored callable called h()
        [doctest2 :DEBUG] Monitored callable called h()
        20
        27
        15
        20

    `profile(self, label=None, level=20)`
    :   Context manager for use with with statement.
        Will profile code within the corresponding with block and generate a log message with the profiling results.

    `reinitialize(self, **kwargs)`
    :   Resets all logger settings to the default values then applies the provided keyword arguments to change attributes
        
        >>> logger = Logger()
        >>> logger.stdout = True
        >>> logger.reinitialize(stderr=False)
        >>> logger.stdout #stdout default is False
        False
        >>> logger.stderr #stderr default is True, but should be overwritten by keyword arg above
        False
        >>> logger.stderr = True

    `set(self, **kwargs)`
    :   Sets attributes based on keyword arguments for this logger (for the lazy who want to write only one line)
        
        Valid for the following attributes:
            auto_debug
            captures_disabled
            debugging_disabled
            disarm_logging_module
            end_interactive
            format
            level
            profiling_disabled
            monitoring_disabled
            stderr
            stdout

    `set_all(self, **kwargs)`
    :   Sets attributes based on keyword arguments for this logger and all of its descendant loggers.
        Valid for the attributes listed in the Logger.set() method's documentation..
        >>> logger = getLogger('doctest')
        >>> grandchild = getLogger('doctest.child.grandchild')
        >>> _=logger.set_all(level=DEBUG)
        >>> grandchild.level == DEBUG
        True
        >>> _=logger.set_all(level=NOTSET)
        >>> grandchild.level == NOTSET
        True

    `set_format(self, fmt=None, time_fmt=None, style=None, handle=None)`
    :   Sets the formatting for handlers.
        The first three arguments are the same as logging.
        
        If handle is None - will apply to all existing handlers and to any that are created in the future.
        
        If handle == "stdout" or "stderr" will apply to one of those streams.
        
        Otherwise, if handle is a FileHandle, StreamHandle, or SpecialHandle object, will apply to the associated handler
        
        >>> logger = Logger('doctest')
        >>> with logger.use(stderr=False,stdout=True):
        ...    logger.set_format('%(levelname)s - %(msg)s')
        ...    logger.warning('hi')
        ... 
        WARNING - hi
        >>> with logger.use(doctest_mode=True,stdout=True):
        ...    logger.warning('bye')
        [doctest :WARNING] bye

    `stream_open(self, stream=None)`
    :   Configures the logger to log to a StringIO object.
        If one is not provided a new one will be created.
        
        Returns a logarhythm.StreamHandle object.
        The StringIO object is present as the stream attribute of the StreamHandle object.
        The StreamHandle object also has a getvalue() method that returns the string context of the StringIO object.
        
        The StreamHandle object can be used as a context manager. When the context block ends, the logger will no longer
        log messages to the StreamHandle
        
        >>> logger = Logger('doctest')
        >>> with logger.use(stdout=False,doctest_mode=True,level=DEBUG):
        ...    with logger.stream_open() as sh:
        ...       logger.debug('hello world')
        ... 
        ...
        >>> print(sh.getvalue().strip())
        [doctest :DEBUG] hello world

    `unmonitor(self, target)`
    :   Removes all monitoring on an object with respect to this logger for which monitor_attr() or monitor_call() was used on.

    `use(self, **kwargs)`
    :   Used in a with block.
        Temporarily sets logger attributes based on keyword arguments, then sets them back at the end of the block.
        Valid for the attributes listed in the Logger.set() method's documentation.

    `walk(self)`
    :   Iterates through this logger and all of its descendants (i.e. children, children's children, etc)

    `warning(self, msg, *args, **kwargs)`
    :   Logs a message at level WARNING. See logging documentation for args and kwargs information.

    `will_log(self, level)`
    :   >>> logger = getLogger()
        >>> logger.will_log(DEBUG)
        False
        >>> root_logger.level = DEBUG
        >>> logger.will_log(DEBUG)
        True
        >>> root_logger.level = NOTSET
        >>> logger.will_log(WARNING)
        True
        >>> logger.will_log(INFO)
        False

`LoggingHandle(logger)`
:   Base class for logging handles (controlling a message output target like a file)

    ### Descendants

    * logarhythm.logarhythm.SpecialHandle
    * logarhythm.logarhythm.StreamHandle
    * logarhythm.logarhythm.FileHandle

    ### Methods

    `close(self)`
    :   Base class method to be overridden by subclasses

`MonitoredAttrItem(*args, **kwargs)`
:   Base class to indicate that an object has had monitor_attr() called on it.

`MonitoredMethItem(*args, **kwargs)`
:   Base class to indicate that an object has had monitor_call() called on one of its methods

`SpecialHandle(logger, handler)`
:   The result of calling logger.handler_add() with some logging module handler

    ### Ancestors (in MRO)

    * logarhythm.logarhythm.LoggingHandle

    ### Methods

    `close(self)`
    :   When closed, the logger will no longer write to this handler.

`StreamHandle(logger, stream)`
:   A handle that represents a StringIO object which has logging messages being written to it

    ### Ancestors (in MRO)

    * logarhythm.logarhythm.LoggingHandle

    ### Methods

    `close(self)`
    :   When closed, the logger will no longer write to this stream.
        
        The underlying StringIO object is not closed/destroyed.

    `getvalue(self)`
    :   Returns the string data in the underlying StringIO object.