run-php
=======

Run PHP function/script with-in nodejs application. This is the `--harmony-proxies`
version of [exec-php](https://www.npmjs.com/package/exec-php). If you're not 
allowed to use such flag, please don't use this project.

Please report any bug somewhere on the github page ( you know where ).

## Installation

    npm install run-php --save

## Usage

    var php = require('run-php');
    
    // set PHP Binary file
    php.binaryFile = '/usr/bin/php';
    
    // get output buffer
    var str = php.eval('echo "a"').ob_get_contents();
    // str == 'a'
    
    // get returned function value
    var len = php.strlen('str');
    // len == 3
    
    // require user file and call user function
    var str = php.require('./file.php').myFunction('lorem');
    // str == 'lorem'
    // in `./file.php` file:
    //    <?php
    //    function myFunction($str){ return $str; }


## Options

### cwd

In case you've some script like `require('./file.php')` on your PHP file, it's always
relative to cwd. You may want to set cwd of you required php file by running:

    php = require('php');
    
    php.options.cwd = '/your/php/base/path';    // Set your php file CWD here
    php.require('./file.php');

### binaryFile

The module need for php binary file to be able to run. Set your system php binary
file with:

    php.binaryFile = '/absolute/path/to/php/binary/file';

## PHP Build-in Function

Some PHP function imported to run-php method. They're:

### eval

This is still evil, but you could use it with a little angel to evaluate your 
php script. It return object your source return.

    php = require('run-php');
    
    var str = php.eval('return "lorem";');
    // str == 'lorem'

### ob_get_contents

Get the last printed string of you php script

    php = require('run-php');
    
    php.eval('echo "a";');
    var str = php.ob_get_contents();
    // str == 'a'

### require

Require your php file within nodejs application. It return `run-php` object.

    php = require('run-php')
    php.require('./file.php')

### run

Run the script. If you're calling script that return `run-php` object, your last
action will be just saved to be executed later. Run `run` method to actually run
them.

    php = require('run-php')
    php.require('./echo-somehthing.php') // save it for next execution.
    php.run();                           // Actually run the php script

## User Functions

After requiring your file, the function is ready to call as if they're `run-php`
method. Thanks to `Proxy`.

    php = require('php')
    php.require('./file-where-you-define-function.php');
    
    // call your function with args.
    php.thisIsMyFunction(1,2,[1,2]);