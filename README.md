# Debugger
### Simple logger and debugger tool for php. All under one script!

## Basic Usage
```php
// Include script
require 'Debugger.php';

// Create a Debugger instance
$debugger = new Debugger('/home/sohel/Documents/Logfile.log');

// Simply print a message, this will not put message in log file
$debugger->msg('Arthur Curry, I hear you can talk to fish.');

// Put some info in log file
$data = "Luke, I am your father!";
$debugger->info($data);

// AND YESS!! There are shortcuts too!
$data = array("Diana" => "He said he'll fight with us?", "Bruce" => "More or less.");
$debugger->i($data); // Same as info()

// Similarly put error or warning into the log
$debugger->warn("So, you're fast?");
$debugger->error("That feels like an over simplification.");

// Pass array or object
$debugger->i($_SERVER);

// Return output instead of putting into log. Pass true as second parameter.
echo $debugger->i("I'm real when it's useful", true); // Prints instead of putting into log
```

## Documentation
### Constructor
* Default constructor will use **/tmp** directory and a file called **Debugger.log**.
* *Note that in many systems http or www-data user data are stored in subdirectories created by apache server.*
```php
$debugger = new Debugger();
```
* Instantiate with path and log file segmentation configurations
```php
$debugger = new Debugger(<file path>, <file segmentation>);
$debugger = new Debugger('/home/sohel/Documents/Logfile.log', 'DAY');
```

### File Segmentation
* Log file segmentation puts a suffix in log file name based on time.
* For example, 'DAY' will add a suffix for a day, like 2016-08-30 and make the file name like **Logfile_2016-08-30.log**
* Suffix is added with the given file name, so if there is a file named **Logfle.log**, a new file will be created with a name **Logfile_2016-08-30.log** if not already exists,
* Other segmentations are 'NONE', 'YEAR', 'MONTH', 'DAY', 'HOUR', 'MINUTE', 'SECOND'
* Default segmentation is 'NONE', which won't create any segments and put all logs in one file.

### Getters and Setters
* You can set file path and file segmentation using setter methods, and these will impact instantly.
```php
$debugger->setPath('/var/www/logs/New.log');
$debugger->setSeg('MINUTE'); // 'NONE', 'YEAR', 'MONTH', 'DAY', 'HOUR', 'MINUTE', 'SECOND'
```
* Get current path and segmentation configuration using getter methods.
```php
$path = $debugger->getPath();
$seg = $debugger->getSeg();
```

### Logging Type
* There are five similar yet different methods
```php
$data = 'I can do this all day!';
$debugger->info($data); // General info, uses print_r() output
$debugger->error($data);
$debugger->warn($data);
$debugger->debug($data); // Extensive debug, uses var_export() output
$debugger->json($_SERVER); // Uses JSON string
```
* All of these methods have short names.

```php
$debugger->i($data);
$debugger->d($data);
$debugger->j($data);
$debugger->e($data);
$debugger->w($data);
```

* Output can be returned instead of putting in log file. Passing **true** as second parameter will return the data.

```php
$output = $debugger->d($data, true);
$output = $debugger->j($data, true);
```

### Output Format
* Output data is a string containing a preamble followed by formatted data.
* Preamble consists of log serial number, timestamp, log type and data type. Preamble looks like following,
> **[2] [2016-09-03 09:35:10] [DEBUG] [DATATYPE:object]**
* When data is returned, by default it's wrapped inside a **pre** tag, just to increase redability on browser.
* Pretty formatting can be disabled and enabled by calling **ugly()** and **pretty()** methods respectively.

```php
$debugger->ugly(); // Disables <pre> wrapper
$debugger->pretty();
```
* json() method has a third parameter, allowing raw output. Raw output does not have any preamble or pretty formatting.
```php
$serverJSON = $debugger->j($_SERVER, true, true); // Returns JSON string of $_SERVER variable.
```

### Counter
* Debugger has a built in counter. Counting starts from zero. Each time calling count() method will increment counter by one.
```php
echo $debugger->count(); // Increments the counter by one
```
* Counter has other actions, like decrement. reset and get current value. Action strings are passed as function parameter. For all actions, counter performs the action first and then return the counter value.
```php
echo $debugger->count('DEC'); // Decrements the counter by one
echo $debugger->count('RESET'); // Resets the counter to zero
echo $debugger->count('GET'); // Get the current counter value
```
### Others
* Print timestamp. Timestamp can be returned by passing **true** as parameter.
```php
$debugger->time();
echo $debugger->time(true);
```
* Print a simple message
```php
$debugger->msg('Winter is coming!');
$debugger->msg(); // This will simply print 'Hello, World!'
```

** That'd be all. Happy debugging! **