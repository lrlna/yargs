examples
========

With yargs, the options be just a hash!
-------------------------------------------------------------------

plunder.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs').argv;

if (argv.ships > 3 && argv.distance < 53.5) {
    console.log('Plunder more riffiwobbles!');
} else {
    console.log('Retreat from the xupptumblers!');
}
````

***

    $ ./plunder.js --ships=4 --distance=22
    Plunder more riffiwobbles!

    $ ./plunder.js --ships 12 --distance 98.7
    Retreat from the xupptumblers!

    ![Joe was one optimistic pirate.](http://i.imgur.com/4WFGVJ9.png)

But don't walk the plank just yet! There be more! You can do short options:
-------------------------------------------------

short.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs').argv;
console.log('(%d,%d)', argv.x, argv.y);
````

***

    $ ./short.js -x 10 -y 21
    (10,21)

And booleans, both long, short, and even grouped:
----------------------------------

bool.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs').argv;

if (argv.s) {
    process.stdout.write(argv.fr ? 'Le perroquet dit: ' : 'The parrot says: ');
}
console.log(
    (argv.fr ? 'couac' : 'squawk') + (argv.p ? '!' : '')
);
````

***

    $ ./bool.js -s
    The parrot says: squawk

    $ ./bool.js -sp
    The parrot says: squawk!

    $ ./bool.js -sp --fr
    Le perroquet dit: couac!

And non-hyphenated options too! Just use `argv._`!
-------------------------------------------------

nonopt.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs').argv;
console.log('(%d,%d)', argv.x, argv.y);
console.log(argv._);
````

***

    $ ./nonopt.js -x 6.82 -y 3.35 rum
    (6.82,3.35)
    [ 'rum' ]

    $ ./nonopt.js "me hearties" -x 0.54 yo -y 1.12 ho
    (0.54,1.12)
    [ 'me hearties', 'yo', 'ho' ]

Yargs even counts your booleans!
----------------------------------------------------------------------

count.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs')
    .count('verbose')
    .alias('v', 'verbose')
    .argv;

VERBOSE_LEVEL = argv.verbose;

function WARN()  { VERBOSE_LEVEL >= 0 && console.log.apply(console, arguments); }
function INFO()  { VERBOSE_LEVEL >= 1 && console.log.apply(console, arguments); }
function DEBUG() { VERBOSE_LEVEL >= 2 && console.log.apply(console, arguments); }

WARN("Showing only important stuff");
INFO("Showing semi-important stuff too");
DEBUG("Extra chatty mode");
````

***
    $ node count.js
    Showing only important stuff

    $ node count.js -v
    Showing only important stuff
    Showing semi-important stuff too

    $ node count.js -vv
    Showing only important stuff
    Showing semi-important stuff too
    Extra chatty mode

    $ node count.js -v --verbose
    Showing only important stuff
    Showing semi-important stuff too
    Extra chatty mode

Tell users how to use yer options and make demands.
-------------------------------------------------

area.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs')
    .usage('Usage: $0 -w [num] -h [num]')
    .demand(['w','h'])
    .argv;

console.log("The area is:", argv.w * argv.h);
````

***

    $ ./area.js -w 55 -h 11
    The area is: 605

    $ node ./area.js -w 4.91 -w 2.51
    Usage: area.js -w [num] -h [num]

    Options:
      -w  [required]
      -h  [required]

    Missing required arguments: h

After yer demands have been met, demand more! Ask for non-hyphenated arguments!
-----------------------------------------

demand_count.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs')
    .demand(2)
    .argv;
console.dir(argv);
````

***

	$ ./demand_count.js a

	Not enough non-option arguments: got 1, need at least 2

	$ ./demand_count.js a b
	{ _: [ 'a', 'b' ], '$0': 'demand_count.js' }

	$ ./demand_count.js a b c
	{ _: [ 'a', 'b', 'c' ], '$0': 'demand_count.js' }

EVEN MORE SHIVER ME TIMBERS!
------------------

default_singles.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs')
    .default('x', 10)
    .default('y', 10)
    .argv
;
console.log(argv.x + argv.y);
````

***

    $ ./default_singles.js -x 5
    15

default_hash.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs')
    .default({ x : 10, y : 10 })
    .argv
;
console.log(argv.x + argv.y);
````

***

    $ ./default_hash.js -y 7
    17

And if you really want to get all descriptive about it...
---------------------------------------------------------

boolean_single.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs')
    .boolean('v')
    .argv
;
console.dir(argv.v);
console.dir(argv._);
````

***

    $ ./boolean_single.js -v "me hearties" yo ho
    true
    [ 'me hearties', 'yo', 'ho' ]


boolean_double.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs')
    .boolean(['x','y','z'])
    .argv
;
console.dir([ argv.x, argv.y, argv.z ]);
console.dir(argv._);
````

***

    $ ./boolean_double.js -x -z one two three
    [ true, false, true ]
    [ 'one', 'two', 'three' ]

Yargs is here to help you...
---------------------------

Ye can describe parameters fer help messages and set aliases. Yargs figures
out how ter format a handy help string automatically.

line_count.js:

````javascript
#!/usr/bin/env node
var argv = require('yargs')
    .usage('Usage: $0 <command> [options]')
    .command('count', 'Count the lines in a file')
    .example('$0 count -f foo.js', 'count the lines in the given file')
    .alias('f', 'file')
    .nargs('f', 1)
    .describe('f', 'Load a file')
    .demand(1, ['f'])
    .help('h')
    .alias('h', 'help')
    .epilog('copyright 2015')
    .argv;

var fs = require('fs');
var s = fs.createReadStream(argv.file);

var lines = 0;
s.on('data', function (buf) {
    lines += buf.toString().match(/\n/g).length;
});

s.on('end', function () {
    console.log(lines);
});
````

***
    $ node line_count.js count
    Usage: line_count.js <command> [options]

    Commands:
      count    Count the lines in a file

    Options:
      -f, --file  Load a file        [required]
      -h, --help  Show help           [boolean]

    Examples:
      line_count.js count -f foo.js  count the lines in the given file

    copyright 2015

    Missing required arguments: f

    $ node line_count.js count --file line_count.js
    26

    $ node line_count.js count -f line_count.js
    26