---
title: Commander
date: 2025-05-11 19:16:12
updated:
tags:
categories:
---


# 开发文档 | Commander 中文网

---

## 安装 

　　Installation

```sh
npm install commander
```

## 快速入门 

　　Quick Start

　　你编写代码来描述你的命令行接口。Commander 将参数解析为选项和命令参数，显示问题的使用错误，并实现帮助系统。

　　You write code to describe your command line interface. Commander looks after parsing the arguments into options and command-arguments, displays usage errors for problems, and implements a help system.

　　Commander 很严格，对无法识别的选项显示错误。最常用的两种选项类型是布尔选项和从以下参数中获取其值的选项。

　　Commander is strict and displays an error for unrecognised options. The two most used option types are a boolean option, and an option which takes its value from the following argument.

　　示例文件：[split.js](https://github.com/tj/commander.js/blob/HEAD/examples/split.js)

　　Example file: [split.js](https://github.com/tj/commander.js/blob/HEAD/examples/split.js)

```js
const { program } = require('commander');

program
  .option('--first')
  .option('-s, --separator <char>')
  .argument('<string>');

program.parse();

const options = program.opts();
const limit = options.first ? 1 : undefined;
console.log(program.args[0].split(options.separator, limit));
```

　　console

```
$ node split.js -s / --fits a/b/c
error: unknown option '--fits'
(Did you mean --first?)
$ node split.js -s / --first a/b/c
[ 'a' ]
```

　　这是一个使用子命令的更完整的程序，并带有帮助说明。在多命令程序中，每个命令都有一个操作处理程序（或命令的独立可执行文件）。

　　Here is a more complete program using a subcommand and with descriptions for the help. In a multi-command program, you have an action handler for each command (or stand-alone executables for the commands).

　　示例文件：[string-util.js](https://github.com/tj/commander.js/blob/HEAD/examples/string-util.js)

　　Example file: [string-util.js](https://github.com/tj/commander.js/blob/HEAD/examples/string-util.js)

　　js

```
const { Command } = require('commander');
const program = new Command();

program
  .name('string-util')
  .description('CLI to some JavaScript string utilities')
  .version('0.8.0');

program.command('split')
  .description('Split a string into substrings and display as an array')
  .argument('<string>', 'string to split')
  .option('--first', 'display just the first substring')
  .option('-s, --separator <char>', 'separator character', ',')
  .action((str, options) => {
    const limit = options.first ? 1 : undefined;
    console.log(str.split(options.separator, limit));
  });

program.parse();
```

　　console

```
$ node string-util.js help split
Usage: string-util split [options] <string>

Split a string into substrings and display as an array.

Arguments:
  string                  string to split

Options:
  --first                 display just the first substring
  -s, --separator <char>  separator character (default: ",")
  -h, --help              display help for command

$ node string-util.js split --separator=/ a/b/c
[ 'a', 'b', 'c' ]
```

　　更多示例可在 [examples](https://github.com/tj/commander.js/tree/master/examples) 目录中找到。

　　More samples can be found in the [examples](https://github.com/tj/commander.js/tree/master/examples) directory.

## 声明程序变量 

　　Declaring *program* variable

　　Commander 导出一个全局对象，这对于快速程序很方便。为简洁起见，此方法用于本 README 中的示例。

　　Commander exports a global object which is convenient for quick programs. This is used in the examples in this README for brevity.

　　js

```
// CommonJS (.cjs)
const { program } = require('commander');
```

　　对于可能以多种方式使用 commander 的大型程序（包括单元测试），最好创建一个本地 Command 对象来使用。

　　For larger programs which may use commander in multiple ways, including unit testing, it is better to create a local Command object to use.

　　js

```
// CommonJS (.cjs)
const { Command } = require('commander');
const program = new Command();
```

　　js

```
// ECMAScript (.mjs)
import { Command } from 'commander';
const program = new Command();
```

　　ts

```
// TypeScript (.ts)
import { Command } from 'commander';
const program = new Command();
```

## 选项 

　　Options

　　选项使用 `.option()`​ 方法定义，也用作选项的文档。每个选项可以有一个短标志（单个字符）和一个长名称，以逗号或空格或竖线（'|'）分隔。为了允许比单个字符更广泛的短标志，你还可以有两个长选项。示例：

　　Options are defined with the `.option()`​ method, also serving as documentation for the options. Each option can have a short flag (single character) and a long name, separated by a comma or space or vertical bar ('|'). To allow a wider range of short-ish flags than just single characters, you may also have two long options. Examples:

　　js

```
program
  .option('-p, --port <number>', 'server port number')
  .option('--trace', 'add extra debugging output')
  .option('--ws, --workspace <name>', 'use a custom workspace')
```

　　可以通过在 `Command`​ 对象上调用 `.opts()`​ 来访问解析的选项，并将其传递给操作处理程序。

　　The parsed options can be accessed by calling `.opts()`​ on a `Command`​ object, and are passed to the action handler.

　　多词选项（例如 "--template-engine"）采用驼峰式大小写，变为 `program.opts().templateEngine`​ 等。

　　Multi-word options such as "--template-engine" are camel-cased, becoming `program.opts().templateEngine`​ etc.

　　选项及其选项参数可以用空格分隔，也可以组合成同一个参数。选项参数可以直接跟在短选项后面，也可以跟在 `=`​ 后面以获得长选项。

　　An option and its option-argument can be separated by a space, or combined into the same argument. The option-argument can follow the short option directly or follow an `=`​ for a long option.

　　sh

```
serve -p 80
serve -p80
serve --port 80
serve --port=80
```

　　你可以使用 `--`​ 来指示选项的结束，并且任何剩余的参数都将被使用而不被解释。

　　You can use `--`​ to indicate the end of the options, and any remaining arguments will be used without being interpreted.

　　默认情况下，命令行上的选项不是位置性的，可以在其他参数之前或之后指定。

　　By default, options on the command line are not positional, and can be specified before or after other arguments.

　　当 `.opts()`​ 不够用时，还有其他相关例程：

　　There are additional related routines for when `.opts()`​ is not enough:

* ​`.optsWithGlobals()`​ 返回合并的本地和全局选项值
  `.optsWithGlobals()`​ returns merged local and global option values
* ​`.getOptionValue()`​ 和 `.setOptionValue()`​ 使用单个选项值
  `.getOptionValue()`​ and `.setOptionValue()`​ work with a single option value
* ​`.getOptionValueSource()`​ 和 `.setOptionValueWithSource()`​ 包括选项值的来源
  `.getOptionValueSource()`​ and `.setOptionValueWithSource()`​ include where the option value came from

### 常见选项类型、布尔值和值 

　　Common option types, boolean and value

　　最常用的两种选项类型是布尔选项和从以下参数中获取其值的选项（用尖括号声明，如 `--expect <value>`​）。除非在命令行上指定，否则两者都是 `undefined`​。

　　The two most used option types are a boolean option, and an option which takes its value from the following argument (declared with angle brackets like `--expect <value>`​). Both are `undefined`​ unless specified on command line.

　　示例文件：[options-common.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-common.js)

　　Example file: [options-common.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-common.js)

　　js

```
program
  .option('-d, --debug', 'output extra debugging')
  .option('-s, --small', 'small pizza size')
  .option('-p, --pizza-type <type>', 'flavour of pizza');

program.parse(process.argv);

const options = program.opts();
if (options.debug) console.log(options);
console.log('pizza details:');
if (options.small) console.log('- small pizza size');
if (options.pizzaType) console.log(`- ${options.pizzaType}`);
```

　　console

```
$ pizza-options -p
error: option '-p, --pizza-type <type>' argument missing
$ pizza-options -d -s -p vegetarian
{ debug: true, small: true, pizzaType: 'vegetarian' }
pizza details:
- small pizza size
- vegetarian
$ pizza-options --pizza-type=cheese
pizza details:
- cheese
```

　　多个布尔短选项可以在破折号后组合，也可以跟一个带有值的短选项。例如，`-d -s -p cheese`​ 可以写为 `-ds -p cheese`​ 甚至 `-dsp cheese`​。

　　Multiple boolean short options may be combined following the dash, and may be followed by a single short option taking a value. For example `-d -s -p cheese`​ may be written as `-ds -p cheese`​ or even `-dsp cheese`​.

　　带有预期选项参数的选项是贪婪的，无论值是什么，都会使用以下参数。因此 `--id -xyz`​ 将 `-xyz`​ 读取为选项参数。

　　Options with an expected option-argument are greedy and will consume the following argument whatever the value. So `--id -xyz`​ reads `-xyz`​ as the option-argument.

　　​`program.parse(arguments)`​ 处理参数，将程序选项未使用的任何参数留在 `program.args`​ 数组中。参数是可选的，默认为 `process.argv`​。

　　`program.parse(arguments)`​ processes the arguments, leaving any args not consumed by the program options in the `program.args`​ array. The parameter is optional and defaults to `process.argv`​.

### 默认选项值 

　　Default option value

　　你可以为选项指定默认值。

　　You can specify a default value for an option.

　　示例文件：[options-defaults.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-defaults.js)

　　Example file: [options-defaults.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-defaults.js)

　　js

```
program
  .option('-c, --cheese <type>', 'add the specified type of cheese', 'blue');

program.parse();

console.log(`cheese: ${program.opts().cheese}`);
```

　　console

```
$ pizza-options
cheese: blue
$ pizza-options --cheese stilton
cheese: stilton
```

### 其他选项类型，可否定布尔值和布尔值|值 

　　Other option types, negatable boolean and boolean|value

　　你可以定义一个布尔选项长名称，并以 `no-`​ 为前导，以便在使用时将选项值设置为 false。单独定义也会使选项默认为真。

　　You can define a boolean option long name with a leading `no-`​ to set the option value to false when used. Defined alone this also makes the option true by default.

　　如果你首先定义 `--foo`​，则添加 `--no-foo`​ 不会改变默认值。

　　If you define `--foo`​ first, adding `--no-foo`​ does not change the default value from what it would otherwise be.

　　示例文件：[options-negatable.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-negatable.js)

　　Example file: [options-negatable.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-negatable.js)

　　js

```
program
  .option('--no-sauce', 'Remove sauce')
  .option('--cheese <flavour>', 'cheese flavour', 'mozzarella')
  .option('--no-cheese', 'plain with no cheese')
  .parse();

const options = program.opts();
const sauceStr = options.sauce ? 'sauce' : 'no sauce';
const cheeseStr = (options.cheese === false) ? 'no cheese' : `${options.cheese} cheese`;
console.log(`You ordered a pizza with ${sauceStr} and ${cheeseStr}`);
```

　　console

```
$ pizza-options
You ordered a pizza with sauce and mozzarella cheese
$ pizza-options --sauce
error: unknown option '--sauce'
$ pizza-options --cheese=blue
You ordered a pizza with sauce and blue cheese
$ pizza-options --no-sauce --no-cheese
You ordered a pizza with no sauce and no cheese
```

　　你可以指定一个选项，该选项可用作布尔选项，但可以选择采用选项参数（用方括号声明，如 `--optional [value]`​）。

　　You can specify an option which may be used as a boolean option but may optionally take an option-argument (declared with square brackets like `--optional [value]`​).

　　示例文件：[options-boolean-or-value.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-boolean-or-value.js)

　　Example file: [options-boolean-or-value.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-boolean-or-value.js)

　　js

```
program
  .option('-c, --cheese [type]', 'Add cheese with optional type');

program.parse(process.argv);

const options = program.opts();
if (options.cheese === undefined) console.log('no cheese');
else if (options.cheese === true) console.log('add cheese');
else console.log(`add cheese type ${options.cheese}`);
```

　　console

```
$ pizza-options
no cheese
$ pizza-options --cheese
add cheese
$ pizza-options --cheese mozzarella
add cheese type mozzarella
```

　　带有可选选项参数的选项不是贪婪的，将忽略以破折号开头的参数。因此 `id`​ 表现为 `--id -5`​ 的布尔选项，但如果需要，你可以使用组合形式，如 `--id=-5`​。

　　Options with an optional option-argument are not greedy and will ignore arguments starting with a dash. So `id`​ behaves as a boolean option for `--id -5`​, but you can use a combined form if needed like `--id=-5`​.

　　有关可能的歧义情况的信息，请参阅 [选项采用不同的参数](https://github.com/tj/commander.js/blob/HEAD/docs/options-in-depth.md)。

　　For information about possible ambiguous cases, see [options taking varying arguments](https://github.com/tj/commander.js/blob/HEAD/docs/options-in-depth.md).

### 必需选项 

　　Required option

　　你可以使用 `.requiredOption()`​ 指定必需（强制）选项。选项在解析后必须有一个值，通常在命令行上指定，或者可能来自默认值（例如来自环境）。该方法在格式上与 `.option()`​ 相同，采用标志和描述，以及可选的默认值或自定义处理。

　　You may specify a required (mandatory) option using `.requiredOption()`​. The option must have a value after parsing, usually specified on the command line, or perhaps from a default value (say from environment). The method is otherwise the same as `.option()`​ in format, taking flags and description, and optional default value or custom processing.

　　示例文件：[options-required.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-required.js)

　　Example file: [options-required.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-required.js)

　　js

```
program
  .requiredOption('-c, --cheese <type>', 'pizza must have cheese');

program.parse();
```

　　console

```
$ pizza
error: required option '-c, --cheese <type>' not specified
```

### 可变参数选项 

　　Variadic option

　　你可以通过在声明选项时将 `...`​ 附加到值占位符来使选项可变。然后，你可以在命令行上指定多个选项参数，解析后的选项值将是一个数组。读取额外的参数，直到第一个参数以破折号开头。特殊参数 `--`​ 完全停止选项处理。如果在与选项相同的参数中指定了值，则不会读取其他值。

　　You may make an option variadic by appending `...`​ to the value placeholder when declaring the option. On the command line you can then specify multiple option-arguments, and the parsed option value will be an array. The extra arguments are read until the first argument starting with a dash. The special argument `--`​ stops option processing entirely. If a value is specified in the same argument as the option then no further values are read.

　　示例文件：[options-variadic.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-variadic.js)

　　Example file: [options-variadic.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-variadic.js)

　　js

```
program
  .option('-n, --number <numbers...>', 'specify numbers')
  .option('-l, --letter [letters...]', 'specify letters');

program.parse();

console.log('Options: ', program.opts());
console.log('Remaining arguments: ', program.args);
```

　　console

```
$ collect -n 1 2 3 --letter a b c
Options:  { number: [ '1', '2', '3' ], letter: [ 'a', 'b', 'c' ] }
Remaining arguments:  []
$ collect --letter=A -n80 operand
Options:  { number: [ '80' ], letter: [ 'A' ] }
Remaining arguments:  [ 'operand' ]
$ collect --letter -n 1 -n 2 3 -- operand
Options:  { number: [ '1', '2', '3' ], letter: true }
Remaining arguments:  [ 'operand' ]
```

　　有关可能的歧义情况的信息，请参阅 [选项采用不同的参数](https://github.com/tj/commander.js/blob/HEAD/docs/options-in-depth.md)。

　　For information about possible ambiguous cases, see [options taking varying arguments](https://github.com/tj/commander.js/blob/HEAD/docs/options-in-depth.md).

### 版本选项 

　　Version option

　　可选的 `version`​ 方法添加了显示命令版本的处理。默认选项标志是 `-V`​ 和 `--version`​，当存在时，命令会打印版本号并退出。

　　The optional `version`​ method adds handling for displaying the command version. The default option flags are `-V`​ and `--version`​, and when present the command prints the version number and exits.

　　js

```
program.version('0.0.1');
```

　　console

```
$ ./examples/pizza -V
0.0.1
```

　　你可以通过将其他参数传递给 `version`​ 方法来更改标志和说明，使用与 `option`​ 方法相同的标志语法。

　　You may change the flags and description by passing additional parameters to the `version`​ method, using the same syntax for flags as the `option`​ method.

　　js

```
program.version('0.0.1', '-v, --vers', 'output the current version');
```

### 更多配置 

　　More configuration

　　你可以使用 `.option()`​ 方法添加大多数选项，但通过为不太常见的情况明确构建 `Option`​，可以使用一些附加功能。

　　You can add most options using the `.option()`​ method, but there are some additional features available by constructing an `Option`​ explicitly for less common cases.

　　示例文件：[options-extra.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-extra.js), [options-env.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-env.js), [options-conflicts.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-conflicts.js), [options-implies.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-implies.js)

　　Example files: [options-extra.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-extra.js), [options-env.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-env.js), [options-conflicts.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-conflicts.js), [options-implies.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-implies.js)

　　js

```
program
  .addOption(new Option('-s, --secret').hideHelp())
  .addOption(new Option('-t, --timeout <delay>', 'timeout in seconds').default(60, 'one minute'))
  .addOption(new Option('-d, --drink <size>', 'drink size').choices(['small', 'medium', 'large']))
  .addOption(new Option('-p, --port <number>', 'port number').env('PORT'))
  .addOption(new Option('--donate [amount]', 'optional donation in dollars').preset('20').argParser(parseFloat))
  .addOption(new Option('--disable-server', 'disables the server').conflicts('port'))
  .addOption(new Option('--free-drink', 'small drink included free ').implies({ drink: 'small' }));
```

　　console

```
$ extra --help
Usage: help [options]

Options:
  -t, --timeout <delay>  timeout in seconds (default: one minute)
  -d, --drink <size>     drink cup size (choices: "small", "medium", "large")
  -p, --port <number>    port number (env: PORT)
  --donate [amount]      optional donation in dollars (preset: "20")
  --disable-server       disables the server
  --free-drink           small drink included free
  -h, --help             display help for command

$ extra --drink huge
error: option '-d, --drink <size>' argument 'huge' is invalid. Allowed choices are small, medium, large.

$ PORT=80 extra --donate --free-drink
Options:  { timeout: 60, donate: 20, port: '80', freeDrink: true, drink: 'small' }

$ extra --disable-server --port 8000
error: option '--disable-server' cannot be used with option '-p, --port <number>'
```

　　使用 `Option`​ 方法 `.makeOptionMandatory()`​ 指定必需（强制）选项。这与 `Command`​ 方法 [.requiredOption()](https://commander.nodejs.cn/docs/#required-option) 匹配。

　　Specify a required (mandatory) option using the `Option`​ method `.makeOptionMandatory()`​. This matches the `Command`​ method [.requiredOption()](https://commander.nodejs.cn/docs/#required-option).

### 自定义选项处理 

　　Custom option processing

　　你可以指定一个函数来自定义处理选项参数。回调函数接收两个参数，用户指定的选项参数和选项的先前值。它返回选项的新值。

　　You may specify a function to do custom processing of option-arguments. The callback function receives two parameters, the user specified option-argument and the previous value for the option. It returns the new value for the option.

　　这允许你将选项参数强制转换为所需类型，或累积值，或进行完全自定义处理。

　　This allows you to coerce the option-argument to the desired type, or accumulate values, or do entirely custom processing.

　　你可以选择在函数参数后指定选项的默认值/起始值。

　　You can optionally specify the default/starting value for the option after the function parameter.

　　示例文件：[options-custom-processing.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-custom-processing.js)

　　Example file: [options-custom-processing.js](https://github.com/tj/commander.js/blob/HEAD/examples/options-custom-processing.js)

　　js

```
function myParseInt(value, dummyPrevious) {
  // parseInt takes a string and a radix
  const parsedValue = parseInt(value, 10);
  if (isNaN(parsedValue)) {
    throw new commander.InvalidArgumentError('Not a number.');
  }
  return parsedValue;
}

function increaseVerbosity(dummyValue, previous) {
  return previous + 1;
}

function collect(value, previous) {
  return previous.concat([value]);
}

function commaSeparatedList(value, dummyPrevious) {
  return value.split(',');
}

program
  .option('-f, --float <number>', 'float argument', parseFloat)
  .option('-i, --integer <number>', 'integer argument', myParseInt)
  .option('-v, --verbose', 'verbosity that can be increased', increaseVerbosity, 0)
  .option('-c, --collect <value>', 'repeatable value', collect, [])
  .option('-l, --list <items>', 'comma separated list', commaSeparatedList)
;

program.parse();

const options = program.opts();
if (options.float !== undefined) console.log(`float: ${options.float}`);
if (options.integer !== undefined) console.log(`integer: ${options.integer}`);
if (options.verbose > 0) console.log(`verbosity: ${options.verbose}`);
if (options.collect.length > 0) console.log(options.collect);
if (options.list !== undefined) console.log(options.list);
```

　　console

```
$ custom -f 1e2
float: 100
$ custom --integer 2
integer: 2
$ custom -v -v -v
verbose: 3
$ custom -c a -c b -c c
[ 'a', 'b', 'c' ]
$ custom --list x,y,z
[ 'x', 'y', 'z' ]
```

## 命令 

　　Commands

　　你可以使用 `.command()`​ 或 `.addCommand()`​ 指定（子）命令。有两种方法可以实现这些：使用附加到命令的操作处理程序，或作为独立的可执行文件（稍后详细介绍）。子命令可以嵌套（[example](https://github.com/tj/commander.js/blob/HEAD/examples/nestedCommands.js)）。

　　You can specify (sub)commands using `.command()`​ or `.addCommand()`​. There are two ways these can be implemented: using an action handler attached to the command, or as a stand-alone executable file (described in more detail later). The subcommands may be nested ([example](https://github.com/tj/commander.js/blob/HEAD/examples/nestedCommands.js)).

　　在 `.command()`​ 的第一个参数中指定命令名称。你可以在命令名称后附加命令参数，或使用 `.argument()`​ 单独指定它们。参数可能是 `<required>`​ 或 `[optional]`​，最后一个参数也可能是 `variadic...`​。

　　In the first parameter to `.command()`​ you specify the command name. You may append the command-arguments after the command name, or specify them separately using `.argument()`​. The arguments may be `<required>`​ or `[optional]`​, and the last argument may also be `variadic...`​.

　　你可以使用 `.addCommand()`​ 将已配置的子命令添加到程序中。

　　You can use `.addCommand()`​ to add an already configured subcommand to the program.

　　例如：

　　For example:

　　js

```
// Command implemented using action handler (description is supplied separately to `.command`)
// Returns new command for configuring.
program
  .command('clone <source> [destination]')
  .description('clone a repository into a newly created directory')
  .action((source, destination) => {
    console.log('clone command called');
  });

// Command implemented using stand-alone executable file, indicated by adding description as second parameter to `.command`.
// Returns `this` for adding more commands.
program
  .command('start <service>', 'start named service')
  .command('stop [service]', 'stop named service, or all if no name supplied');

// Command prepared separately.
// Returns `this` for adding more commands.
program
  .addCommand(build.makeBuildCommand());
```

　　配置选项可以通过调用 `.command()`​ 和 `.addCommand()`​ 传递。指定 `hidden: true`​ 将从生成的帮助输出中删除该命令。如果没有指定其他子命令 ([example](https://github.com/tj/commander.js/blob/HEAD/examples/defaultCommand.js))，则指定 `isDefault: true`​ 将运行子命令。

　　Configuration options can be passed with the call to `.command()`​ and `.addCommand()`​. Specifying `hidden: true`​ will remove the command from the generated help output. Specifying `isDefault: true`​ will run the subcommand if no other subcommand is specified ([example](https://github.com/tj/commander.js/blob/HEAD/examples/defaultCommand.js)).

　　你可以使用 `.alias()`​ 为命令添加替代名称。([example](https://github.com/tj/commander.js/blob/HEAD/examples/alias.js))

　　You can add alternative names for a command with `.alias()`​. ([example](https://github.com/tj/commander.js/blob/HEAD/examples/alias.js))

　　​`.command()`​ 会自动将继承的设置从父命令复制到新创建的子命令。这仅在创建期间完成，任何后续对父级的设置更改都不会被继承。

　　`.command()`​ automatically copies the inherited settings from the parent command to the newly created subcommand. This is only done during creation, any later setting changes to the parent are not inherited.

　　为了安全起见，`.addCommand()`​ 不会自动复制从父命令继承的设置。有一个辅助例程 `.copyInheritedSettings()`​，用于在需要时复制设置。

　　For safety, `.addCommand()`​ does not automatically copy the inherited settings from the parent command. There is a helper routine `.copyInheritedSettings()`​ for copying the settings when they are wanted.

### 命令参数 

　　Command-arguments

　　对于子命令，你可以在对 `.command()`​ 的调用中指定参数语法（如上所示）。这是使用独立可执行文件实现的子命令唯一可用的方法，但对于其他子命令，你可以改用以下方法。

　　For subcommands, you can specify the argument syntax in the call to `.command()`​ (as shown above). This is the only method usable for subcommands implemented using a stand-alone executable, but for other subcommands you can instead use the following method.

　　要配置命令，可以使用 `.argument()`​ 指定每个预期的命令参数。你提供参数名称和可选描述。参数可能是 `<required>`​ 或 `[optional]`​。你可以为可选命令参数指定默认值。

　　To configure a command, you can use `.argument()`​ to specify each expected command-argument. You supply the argument name and an optional description. The argument may be `<required>`​ or `[optional]`​. You can specify a default value for an optional command-argument.

　　示例文件：[argument.js](https://github.com/tj/commander.js/blob/HEAD/examples/argument.js)

　　Example file: [argument.js](https://github.com/tj/commander.js/blob/HEAD/examples/argument.js)

　　js

```
program
  .version('0.1.0')
  .argument('<username>', 'user to login')
  .argument('[password]', 'password for user, if required', 'no password given')
  .action((username, password) => {
    console.log('username:', username);
    console.log('password:', password);
  });
```

　　命令的最后一个参数可以是可变的，并且只能是最后一个参数。要使参数可变，请在参数名称后附加 `...`​。可变参数作为数组传递给操作处理程序。例如：

　　The last argument of a command can be variadic, and only the last argument. To make an argument variadic you append `...`​ to the argument name. A variadic argument is passed to the action handler as an array. For example:

　　js

```
program
  .version('0.1.0')
  .command('rmdir')
  .argument('<dirs...>')
  .action(function (dirs) {
    dirs.forEach((dir) => {
      console.log('rmdir %s', dir);
    });
  });
```

　　有一种方便的方法可以一次添加多个参数，但没有描述：

　　There is a convenience method to add multiple arguments at once, but without descriptions:

　　js

```
program
  .arguments('<username> <password>');
```

#### 更多配置 

　　More configuration

　　通过明确构建 `Argument`​ 来获得一些不太常见的情况的附加功能。

　　There are some additional features available by constructing an `Argument`​ explicitly for less common cases.

　　示例文件：[arguments-extra.js](https://github.com/tj/commander.js/blob/HEAD/examples/arguments-extra.js)

　　Example file: [arguments-extra.js](https://github.com/tj/commander.js/blob/HEAD/examples/arguments-extra.js)

　　js

```
program
  .addArgument(new commander.Argument('<drink-size>', 'drink cup size').choices(['small', 'medium', 'large']))
  .addArgument(new commander.Argument('[timeout]', 'timeout in seconds').default(60, 'one minute'))
```

#### 自定义参数处理 

　　Custom argument processing

　　你可以指定一个函数来自定义处理命令参数（如选项参数）。回调函数接收两个参数，用户指定的命令参数和参数的先前值。它返回参数的新值。

　　You may specify a function to do custom processing of command-arguments (like for option-arguments). The callback function receives two parameters, the user specified command-argument and the previous value for the argument. It returns the new value for the argument.

　　处理后的参数值传递给操作处理程序，并保存为 `.processedArgs`​。

　　The processed argument values are passed to the action handler, and saved as `.processedArgs`​.

　　你可以选择在函数参数后指定参数的默认值/起始值。

　　You can optionally specify the default/starting value for the argument after the function parameter.

　　示例文件：[arguments-custom-processing.js](https://github.com/tj/commander.js/blob/HEAD/examples/arguments-custom-processing.js)

　　Example file: [arguments-custom-processing.js](https://github.com/tj/commander.js/blob/HEAD/examples/arguments-custom-processing.js)

　　js

```
program
  .command('add')
  .argument('<first>', 'integer argument', myParseInt)
  .argument('[second]', 'integer argument', myParseInt, 1000)
  .action((first, second) => {
    console.log(`${first} + ${second} = ${first + second}`);
  })
;
```

### 动作处理程序 

　　Action handler

　　动作处理程序会为你声明的每个命令参数传递一个参数，以及两个附加参数，即解析的选项和命令对象本身。

　　The action handler gets passed a parameter for each command-argument you declared, and two additional parameters which are the parsed options and the command object itself.

　　示例文件：[thank.js](https://github.com/tj/commander.js/blob/HEAD/examples/thank.js)

　　Example file: [thank.js](https://github.com/tj/commander.js/blob/HEAD/examples/thank.js)

　　js

```
program
  .argument('<name>')
  .option('-t, --title <honorific>', 'title to use before name')
  .option('-d, --debug', 'display some debugging')
  .action((name, options, command) => {
    if (options.debug) {
      console.error('Called %s with options %o', command.name(), options);
    }
    const title = options.title ? `${options.title} ` : '';
    console.log(`Thank-you ${title}${name}`);
  });
```

　　如果你愿意，你可以直接使用命令并跳过声明操作处理程序的参数。`this`​ 关键字设置为正在运行的命令，可以从函数表达式中使用（但不能从箭头函数中使用）。

　　If you prefer, you can work with the command directly and skip declaring the parameters for the action handler. The `this`​ keyword is set to the running command and can be used from a function expression (but not from an arrow function).

　　示例文件：[action-this.js](https://github.com/tj/commander.js/blob/HEAD/examples/action-this.js)

　　Example file: [action-this.js](https://github.com/tj/commander.js/blob/HEAD/examples/action-this.js)

　　js

```
program
  .command('serve')
  .argument('<script>')
  .option('-p, --port <number>', 'port number', 80)
  .action(function() {
    console.error('Run script %s on port %s', this.args[0], this.opts().port);
  });
```

　　你可以提供 `async`​ 操作处理程序，在这种情况下，你调用 `.parseAsync`​ 而不是 `.parse`​。

　　You may supply an `async`​ action handler, in which case you call `.parseAsync`​ rather than `.parse`​.

　　js

```
async function run() { /* code goes here */ }

async function main() {
  program
    .command('run')
    .action(run);
  await program.parseAsync(process.argv);
}
```

　　使用命令时，将验证命令行上的命令选项和参数。任何未知选项或缺失参数或多余参数都将报告为错误。你可以使用 `.allowUnknownOption()`​ 抑制未知选项检查。你可以使用 `.allowExcessArguments()`​ 抑制多余参数检查。

　　A command's options and arguments on the command line are validated when the command is used. Any unknown options or missing arguments or excess arguments will be reported as an error. You can suppress the unknown option check with `.allowUnknownOption()`​. You can suppress the excess arguments check with `.allowExcessArguments()`​.

### 独立可执行（子）命令 

　　Stand-alone executable (sub)commands

　　当使用描述参数调用 `.command()`​ 时，这会告诉 Commander 你将使用独立的可执行文件执行子命令。Commander 将在入口脚本目录中的文件中搜索名称组合为 `command-subcommand`​ 的文件，如下例中的 `pm-install`​ 或 `pm-search`​。搜索包括尝试常见的文件扩展名，如 `.js`​。你可以使用 `executableFile`​ 配置选项指定自定义名称（和路径）。你可以使用 `.executableDir()`​ 为子命令指定自定义搜索目录。

　　When `.command()`​ is invoked with a description argument, this tells Commander that you're going to use stand-alone executables for subcommands. Commander will search the files in the directory of the entry script for a file with the name combination `command-subcommand`​, like `pm-install`​ or `pm-search`​ in the example below. The search includes trying common file extensions, like `.js`​. You may specify a custom name (and path) with the `executableFile`​ configuration option. You may specify a custom search directory for subcommands with `.executableDir()`​.

　　你在可执行文件中处理可执行（子）命令的选项，并且不要在顶层声明它们。

　　You handle the options for an executable (sub)command in the executable, and don't declare them at the top-level.

　　示例文件：[pm](https://github.com/tj/commander.js/blob/HEAD/examples/pm)

　　Example file: [pm](https://github.com/tj/commander.js/blob/HEAD/examples/pm)

　　js

```
program
  .name('pm')
  .version('0.1.0')
  .command('install [package-names...]', 'install one or more packages')
  .command('search [query]', 'search with optional query')
  .command('update', 'update installed packages', { executableFile: 'myUpdateSubCommand' })
  .command('list', 'list packages installed', { isDefault: true });

program.parse(process.argv);
```

　　如果程序设计为全局安装，请确保可执行文件具有适当的模式，例如 `755`​。

　　If the program is designed to be installed globally, make sure the executables have proper modes, like `755`​.

### 生命周期钩子 

　　Life cycle hooks

　　你可以为生命周期事件向命令添加回调钩子。

　　You can add callback hooks to a command for life cycle events.

　　示例文件：[hook.js](https://github.com/tj/commander.js/blob/HEAD/examples/hook.js)

　　Example file: [hook.js](https://github.com/tj/commander.js/blob/HEAD/examples/hook.js)

　　js

```
program
  .option('-t, --trace', 'display trace statements for commands')
  .hook('preAction', (thisCommand, actionCommand) => {
    if (thisCommand.opts().trace) {
      console.log(`About to call action handler for subcommand: ${actionCommand.name()}`);
      console.log('arguments: %O', actionCommand.args);
      console.log('options: %o', actionCommand.opts());
    }
  });
```

　　回调钩子可以是 `async`​，在这种情况下，你调用 `.parseAsync`​ 而不是 `.parse`​。你可以为每个事件添加多个钩子。

　　The callback hook can be `async`​, in which case you call `.parseAsync`​ rather than `.parse`​. You can add multiple hooks per event.

　　支持的事件为：

　　The supported events are:

| 事件名称                    | 当调用钩子时                           | 回调参数                         |
| --------------------------- | -------------------------------------- | -------------------------------- |
| ​`preAction`​,`postAction`​ | 此命令及其嵌套子命令的前后操作处理程序 | ​`(thisCommand, actionCommand)`​ |
| ​`preSubcommand`​           | 在解析直接子命令之前                   | ​`(thisCommand, subcommand)`​    |

　　有关生命周期事件的概述，请参阅 [解析生命周期和钩子](https://github.com/tj/commander.js/blob/HEAD/docs/parsing-and-hooks.md)。

　　For an overview of the life cycle events see [parsing life cycle and hooks](https://github.com/tj/commander.js/blob/HEAD/docs/parsing-and-hooks.md).

## 自动帮助 

　　Automated help

　　帮助信息是根据命令器已经了解的有关你的程序的信息自动生成的。默认帮助选项是 `-h,--help`​。

　　The help information is auto-generated based on the information commander already knows about your program. The default help option is `-h,--help`​.

　　示例文件：[pizza](https://github.com/tj/commander.js/blob/HEAD/examples/pizza)

　　Example file: [pizza](https://github.com/tj/commander.js/blob/HEAD/examples/pizza)

　　console

```
$ node ./examples/pizza --help
Usage: pizza [options]

An application for pizza ordering

Options:
  -p, --peppers        Add peppers
  -c, --cheese <type>  Add the specified type of cheese (default: "marble")
  -C, --no-cheese      You do not want any cheese
  -h, --help           display help for command
```

　　如果你的命令有子命令，则默认会添加 `help`​ 命令。它可以单独使用，也可以与子命令名称一起使用以显示子命令的进一步帮助。如果 `shell`​ 程序具有隐式帮助，则这些实际上是相同的：

　　A `help`​ command is added by default if your command has subcommands. It can be used alone, or with a subcommand name to show further help for the subcommand. These are effectively the same if the `shell`​ program has implicit help:

　　sh

```
shell help
shell --help

shell help spawn
shell spawn --help
```

　　长描述会换行以适应可用宽度。（但是，包含换行符和空格的描述被认为是预格式化的，而不是换行的。）

　　Long descriptions are wrapped to fit the available width. (However, a description that includes a line-break followed by whitespace is assumed to be pre-formatted and not wrapped.)

### 自定义帮助 

　　Custom help

　　你可以添加额外的文本以与内置帮助一起显示。

　　You can add extra text to be displayed along with the built-in help.

　　示例文件：[custom-help](https://github.com/tj/commander.js/blob/HEAD/examples/custom-help)

　　Example file: [custom-help](https://github.com/tj/commander.js/blob/HEAD/examples/custom-help)

　　js

```
program
  .option('-f, --foo', 'enable some foo');

program.addHelpText('after', `

Example call:
  $ custom-help --help`);
```

　　产生以下帮助输出：

　　Yields the following help output:

　　Text

```
Usage: custom-help [options]

Options:
  -f, --foo   enable some foo
  -h, --help  display help for command

Example call:
  $ custom-help --help
```

　　显示顺序的位置为：

　　The positions in order displayed are:

* ​`beforeAll`​：添加到程序中作为全局横幅或标题
  `beforeAll`​: add to the program for a global banner or header
* ​`before`​：在内置帮助前显示额外信息
  `before`​: display extra information before built-in help
* ​`after`​：在内置帮助后显示额外信息
  `after`​: display extra information after built-in help
* ​`afterAll`​：添加到程序中作为全局页脚（结尾）
  `afterAll`​: add to the program for a global footer (epilog)

　　位置 "beforeAll" 和 "afterAll" 适用于命令及其所有子命令。

　　The positions "beforeAll" and "afterAll" apply to the command and all its subcommands.

　　第二个参数可以是字符串，也可以是返回字符串的函数。为方便起见，该函数传递了一个上下文对象。属性为：

　　The second parameter can be a string, or a function returning a string. The function is passed a context object for your convenience. The properties are:

* 错误：一个布尔值，表示是否由于使用错误而显示帮助
  error: a boolean for whether the help is being displayed due to a usage error
* 命令：显示帮助的命令
  command: the Command which is displaying the help

### 出现错误后显示帮助 

　　Display help after errors

　　使用错误的默认行为是仅显示一条简短的错误消息。你可以更改行为以在错误后显示完整帮助或自定义帮助消息。

　　The default behaviour for usage errors is to just display a short error message. You can change the behaviour to show the full help or a custom help message after an error.

　　js

```
program.showHelpAfterError();
// or
program.showHelpAfterError('(add --help for additional information)');
```

　　console

```
$ pizza --unknown
error: unknown option '--unknown'
(add --help for additional information)
```

　　默认行为是在未知命令或选项出错后建议正确的拼写。你可以禁用此功能。

　　The default behaviour is to suggest correct spelling after an error for an unknown command or option. You can disable this.

　　js

```
program.showSuggestionAfterError(false);
```

　　console

```
$ pizza --hepl
error: unknown option '--hepl'
(Did you mean --help?)
```

### 显示代码帮助 

　　Display help from code

　　​`.help()`​：显示帮助信息并立即退出。你可以选择传递 `{ error: true }`​ 以在 stderr 上显示并以错误状态退出。

　　`.help()`​: display help information and exit immediately. You can optionally pass `{ error: true }`​ to display on stderr and exit with an error status.

　　​`.outputHelp()`​：输出帮助信息而不退出。你可以选择传递 `{ error: true }`​ 以在 stderr 上显示。

　　`.outputHelp()`​: output help information without exiting. You can optionally pass `{ error: true }`​ to display on stderr.

　　​`.helpInformation()`​：获取内置命令帮助信息作为字符串，以便自己处理或显示。

　　`.helpInformation()`​: get the built-in command help information as a string for processing or displaying yourself.

### .name 

　　命令名称出现在帮助中，也用于定位独立的可执行子命令。

　　The command name appears in the help, and is also used for locating stand-alone executable subcommands.

　　你可以使用 `.name()`​ 或在 Command 构造函数中指定程序名称。对于程序，Commander 将回退到使用传递给 `.parse()`​ 的完整参数中的脚本名称。但是，脚本名称会根据程序的启动方式而有所不同，因此你可能希望明确指定它。

　　You may specify the program name using `.name()`​ or in the Command constructor. For the program, Commander will fall back to using the script name from the full arguments passed into `.parse()`​. However, the script name varies depending on how your program is launched, so you may wish to specify it explicitly.

　　js

```
program.name('pizza');
const pm = new Command('pm');
```

　　使用 `.command()`​ 指定时，子命令会获得名称。如果你自己创建子命令以与 `.addCommand()`​ 一起使用，则使用 `.name()`​ 或在 Command 构造函数中设置名称。

　　Subcommands get a name when specified using `.command()`​. If you create the subcommand yourself to use with `.addCommand()`​, then set the name using `.name()`​ or in the Command constructor.

### .usage 

　　这允许你在帮助的第一行中自定义使用说明。给定：

　　This allows you to customise the usage description in the first line of the help. Given:

　　js

```
program
  .name("my-command")
  .usage("[global options] command")
```

　　帮助将以以下内容开头：

　　The help will start with:

　　Text

```
Usage: my-command [global options] command
```

### .description 和 .summary 

　　.description and .summary

　　描述出现在命令的帮助中。你可以选择在列为程序的子命令时提供更短的摘要。

　　The description appears in the help for the command. You can optionally supply a shorter summary to use when listed as a subcommand of the program.

　　js

```
program
  .command("duplicate")
  .summary("make a copy")
  .description(`Make a copy of the current project.
This may require additional disk space.
  `);
```

### .helpOption(flags, description) 

　　默认情况下，每个命令都有一个帮助选项。你可以更改默认帮助标志和说明。传递 false 以禁用内置帮助选项。

　　By default, every command has a help option. You may change the default help flags and description. Pass false to disable the built-in help option.

　　js

```
program
  .helpOption('-e, --HELP', 'read more information');
```

　　（或者使用 `.addHelpOption()`​ 添加你自己构建的选项。）

　　(Or use `.addHelpOption()`​ to add an option you construct yourself.)

### .helpCommand() 

　　如果你的命令有子命令，则默认会添加帮助命令。你可以使用 `.helpCommand(true)`​ 和 `.helpCommand(false)`​ 明确打开或关闭隐式帮助命令。

　　A help command is added by default if your command has subcommands. You can explicitly turn on or off the implicit help command with `.helpCommand(true)`​ and `.helpCommand(false)`​.

　　你可以通过提供名称和描述来打开和自定义帮助命令：

　　You can both turn on and customise the help command by supplying the name and description:

　　js

```
program.helpCommand('assist [command]', 'show assistance');
```

　　（或者使用 `.addHelpCommand()`​ 添加你自己构建的命令。）

　　(Or use `.addHelpCommand()`​ to add a command you construct yourself.)

### 更多配置 

　　More configuration

　　内置帮助使用 Help 类进行格式化。你可以通过使用 `.configureHelp()`​ 修改数据属性和方法来配置帮助，或者通过使用 `.createHelp()`​ 将 Help 子类化。

　　The built-in help is formatted using the Help class. You can configure the help by modifying data properties and methods using `.configureHelp()`​, or by subclassing Help using `.createHelp()`​ .

　　简单属性包括 `sortSubcommands`​、`sortOptions`​ 和 `showGlobalOptions`​。你可以使用 `styleTitle()`​ 等样式方法添加颜色。

　　Simple properties include `sortSubcommands`​, `sortOptions`​, and `showGlobalOptions`​. You can add color using the style methods like `styleTitle()`​.

　　有关更改显示文本、颜色和布局的更多详细信息和示例，请参阅 (./docs/help-in-depth.md)

　　For more detail and examples of changing the displayed text, color, and layout see (./docs/help-in-depth.md)

## 自定义事件监听器 

　　Custom event listeners

　　你可以通过监听命令和选项事件来执行自定义操作。

　　You can execute custom actions by listening to command and option events.

　　js

```
program.on('option:verbose', function () {
  process.env.VERBOSE = this.opts().verbose;
});
```

## 零碎信息 

　　Bits and pieces

### .parse() and .parseAsync() 

　　不使用参数调用来解析 `process.argv`​。检测 Electron 和特殊节点选项，如 `node --eval`​。简易模式！

　　Call with no parameters to parse `process.argv`​. Detects Electron and special node options like `node --eval`​. Easy mode!

　　或者使用要解析的字符串数组进行调用，并可选择通过指定参数所在的位置 `from`​ 来指定用户参数的起始位置：

　　Or call with an array of strings to parse, and optionally where the user arguments start by specifying where the arguments are `from`​:

* ​`'node'`​：默认情况下，`argv[0]`​ 是应用，`argv[1]`​ 是正在运行的脚本，之后是用户参数
  `'node'`​: default, `argv[0]`​ is the application and `argv[1]`​ is the script being run, with user arguments after that
* ​`'electron'`​：`argv[0]`​ 是应用，`argv[1]`​ 会根据电子应用是否打包而变化
  `'electron'`​: `argv[0]`​ is the application and `argv[1]`​ varies depending on whether the electron application is packaged
* ​`'user'`​：仅用户参数
  `'user'`​: just user arguments

　　例如：

　　For example:

　　js

```
program.parse(); // parse process.argv and auto-detect electron and special node flags
program.parse(process.argv); // assume argv[0] is app and argv[1] is script
program.parse(['--port', '80'], { from: 'user' }); // just user supplied arguments, nothing special about argv[0]
```

　　如果你的任何操作处理程序是异步的，请使用 parseAsync 而不是 parse。

　　Use parseAsync instead of parse if any of your action handlers are async.

### 解析配置 

　　Parsing Configuration

　　如果默认解析不符合你的需求，则有一些行为可以支持其他使用模式。

　　If the default parsing does not suit your needs, there are some behaviours to support other usage patterns.

　　默认情况下，程序选项在子命令之前和之后被识别。要仅在子命令之前查找程序选项，请使用 `.enablePositionalOptions()`​。这允许你在子命令中将选项用于不同的目的。

　　By default, program options are recognised before and after subcommands. To only look for program options before subcommands, use `.enablePositionalOptions()`​. This lets you use an option for a different purpose in subcommands.

　　示例文件：[positional-options.js](https://github.com/tj/commander.js/blob/HEAD/examples/positional-options.js)

　　Example file: [positional-options.js](https://github.com/tj/commander.js/blob/HEAD/examples/positional-options.js)

　　对于位置选项，`-b`​ 是第一行中的程序选项，在第二行中是子命令选项：

　　With positional options, the `-b`​ is a program option in the first line and a subcommand option in the second line:

　　sh

```
program -b subcommand
program subcommand -b
```

　　默认情况下，选项在命令参数之前和之后被识别。要仅处理命令参数之前的选项，请使用 `.passThroughOptions()`​。这允许你将参数和后续选项传递给另一个程序，而无需使用 `--`​ 来结束选项处理。要在子命令中使用传递选项，程序需要启用位置选项。

　　By default, options are recognised before and after command-arguments. To only process options that come before the command-arguments, use `.passThroughOptions()`​. This lets you pass the arguments and following options through to another program without needing to use `--`​ to end the option processing. To use pass through options in a subcommand, the program needs to enable positional options.

　　示例文件：[pass-through-options.js](https://github.com/tj/commander.js/blob/HEAD/examples/pass-through-options.js)

　　Example file: [pass-through-options.js](https://github.com/tj/commander.js/blob/HEAD/examples/pass-through-options.js)

　　对于传递选项，`--port=80`​ 是第一行中的程序选项，并在第二行中作为命令参数传递：

　　With pass through options, the `--port=80`​ is a program option in the first line and passed through as a command-argument in the second line:

　　sh

```
program --port=80 arg
program arg --port=80
```

　　默认情况下，选项处理会显示未知选项的错误。要将未知选项视为普通命令参数并继续查找选项，请使用 `.allowUnknownOption()`​。这允许你混合已知和未知选项。

　　By default, the option processing shows an error for an unknown option. To have an unknown option treated as an ordinary command-argument and continue looking for options, use `.allowUnknownOption()`​. This lets you mix known and unknown options.

　　默认情况下，参数处理不会显示比预期更多的命令参数的错误。要显示多余参数的错误，请使用 `.allowExcessArguments(false)`​。

　　By default, the argument processing does not display an error for more command-arguments than expected. To display an error for excess arguments, use`.allowExcessArguments(false)`​.

### 旧选项作为属性 

　　Legacy options as properties

　　在 Commander 7 之前，选项值存储为命令上的属性。这很方便编码，但缺点是可能与 `Command`​ 的现有属性发生冲突。你可以使用 `.storeOptionsAsProperties()`​ 恢复到旧行为以运行未修改的旧版代码。

　　Before Commander 7, the option values were stored as properties on the command. This was convenient to code, but the downside was possible clashes with existing properties of `Command`​. You can revert to the old behaviour to run unmodified legacy code by using `.storeOptionsAsProperties()`​.

　　js

```
program
  .storeOptionsAsProperties()
  .option('-d, --debug')
  .action((commandAndOptions) => {
    if (commandAndOptions.debug) {
      console.error(`Called ${commandAndOptions.name()}`);
    }
  });
```

### TypeScript 

　　额外输入：有一个可选项目可以从选项和参数定义中推断出额外的类型信息。这会为 `.opts()`​ 返回的选项和 `.action()`​ 的参数添加强类型。有关详细信息，请参阅 [commander-js/extra-typings](https://github.com/commander-js/extra-typings)。

　　extra-typings: There is an optional project to infer extra type information from the option and argument definitions. This adds strong typing to the options returned by `.opts()`​ and the parameters to `.action()`​. See [commander-js/extra-typings](https://github.com/commander-js/extra-typings) for more.

```
import { Command } from '@commander-js/extra-typings';
```

　　ts-node：如果你使用 `ts-node`​ 和以 `.ts`​ 文件形式编写的独立可执行子命令，则需要通过 node 调用程序以正确调用子命令。例如

　　ts-node: If you use `ts-node`​ and stand-alone executable subcommands written as `.ts`​ files, you need to call your program through node to get the subcommands called correctly. e.g.

　　sh

```
node -r ts-node/register pm.ts
```

### createCommand() 

　　此工厂函数创建一个新命令。它已导出，可以代替使用 `new`​，例如：

　　This factory function creates a new command. It is exported and may be used instead of using `new`​, like:

　　js

```
const { createCommand } = require('commander');
const program = createCommand();
```

　　​`createCommand`​ 也是 Command 对象的一种方法，它创建一个新命令而不是子命令。使用 `.command()`​ 创建子命令时，这会在内部使用，你可以覆盖它以自定义新的子命令（示例文件 [custom-command-class.js](https://github.com/tj/commander.js/blob/HEAD/examples/custom-command-class.js)）。

　　`createCommand`​ is also a method of the Command object, and creates a new command rather than a subcommand. This gets used internally when creating subcommands using `.command()`​, and you may override it to customise the new subcommand (example file [custom-command-class.js](https://github.com/tj/commander.js/blob/HEAD/examples/custom-command-class.js)).

### Node 选项，例如 `--harmony`​ 

　　Node options such as `--harmony`​

　　你可以通过两种方式启用 `--harmony`​ 选项：

　　You can enable `--harmony`​ option in two ways:

* 在子命令脚本中使用 `#! /usr/bin/env node --harmony`​。（注意 Windows 不支持此模式。）
  Use `#! /usr/bin/env node --harmony`​ in the subcommands scripts. (Note Windows does not support this pattern.)
* 调用命令时使用 `--harmony`​ 选项，如 `node --harmony examples/pm publish`​。在生成子命令进程时，`--harmony`​ 选项将被保留。
  Use the `--harmony`​ option when call the command, like `node --harmony examples/pm publish`​. The `--harmony`​ option will be preserved when spawning subcommand process.

### 调试独立可执行子命令 

　　Debugging stand-alone executable subcommands

　　可执行子命令作为单独的子进程启动。

　　An executable subcommand is launched as a separate child process.

　　如果你使用 `node --inspect`​ 等对 [debugging](https://nodejs.cn/docs/guides/debugging-getting-started/) 可执行子命令使用 node 检查器，则生成的子命令的检查器端口将增加 1。

　　If you are using the node inspector for [debugging](https://nodejs.cn/docs/guides/debugging-getting-started/) executable subcommands using `node --inspect`​ et al., the inspector port is incremented by 1 for the spawned subcommand.

　　如果你使用 VSCode 调试可执行子命令，则需要在 launch.json 配置中设置 `"autoAttachChildProcesses": true`​ 标志。

　　If you are using VSCode to debug executable subcommands you need to set the `"autoAttachChildProcesses": true`​ flag in your launch.json configuration.

### npm run-script 

　　默认情况下，当你使用运行脚本调用程序时，`npm`​ 将解析命令行上的任何选项，并且它们不会到达你的程序。使用 `--`​ 停止 npm 选项解析并传递所有参数。

　　By default, when you call your program using run-script, `npm`​ will parse any options on the command-line and they will not reach your program. Use `--`​ to stop the npm option parsing and pass through all the arguments.

　　[npm run-script](https://npm.nodejs.cn/cli/v9/commands/npm-run-script) 的概要明确显示了 `--`​，原因如下：

　　The synopsis for [npm run-script](https://npm.nodejs.cn/cli/v9/commands/npm-run-script) explicitly shows the `--`​ for this reason:

　　console

```
npm run-script <command> [-- <args>]
```

### 显示错误 

　　Display error

　　此例程可用于针对你自己的错误情况调用 Commander 错误处理。（另请参阅下一节有关退出处理的内容。）

　　This routine is available to invoke the Commander error handling for your own error conditions. (See also the next section about exit handling.)

　　除了错误消息，你还可以选择指定 `exitCode`​（与 `process.exit`​ 一起使用）和 `code`​（与 `CommanderError`​ 一起使用）。

　　As well as the error message, you can optionally specify the `exitCode`​ (used with `process.exit`​) and `code`​ (used with `CommanderError`​).

　　js

```
program.error('Password must be longer than four characters');
program.error('Custom processing has failed', { exitCode: 2, code: 'my.custom.error' });
```

### 覆盖退出和输出处理 

　　Override exit and output handling

　　默认情况下，Commander 在检测到错误时或在显示帮助或版本后调用 `process.exit`​。你可以覆盖此行为并可选择提供回调。默认覆盖会抛出 `CommanderError`​。

　　By default, Commander calls `process.exit`​ when it detects errors, or after displaying the help or version. You can override this behaviour and optionally supply a callback. The default override throws a `CommanderError`​.

　　覆盖回调传递了一个具有属性 `exitCode`​ 数字、`code`​ 字符串和 `message`​ 的 `CommanderError`​。Commander 期望回调终止正常程序流，并在回调返回时调用 `process.exit`​。错误消息、版本或帮助的正常显示不受显示后调用的覆盖的影响。

　　The override callback is passed a `CommanderError`​ with properties `exitCode`​ number, `code`​ string, and `message`​. Commander expects the callback to terminate the normal program flow, and will call `process.exit`​ if the callback returns. The normal display of error messages or version or help is not affected by the override which is called after the display.

　　js

```
program.exitOverride();

try {
  program.parse(process.argv);
} catch (err) {
  // custom processing...
}
```

　　默认情况下，Commander 配置为命令行应用并写入 stdout 和 stderr。你可以为自定义应用修改此行为。此外，你可以修改错误消息的显示。

　　By default, Commander is configured for a command-line application and writes to stdout and stderr. You can modify this behaviour for custom applications. In addition, you can modify the display of error messages.

　　示例文件：[configure-output.js](https://github.com/tj/commander.js/blob/HEAD/examples/configure-output.js)

　　Example file: [configure-output.js](https://github.com/tj/commander.js/blob/HEAD/examples/configure-output.js)

　　js

```
function errorColor(str) {
  // Add ANSI escape codes to display text in red.
  return `\x1b[31m${str}\x1b[0m`;
}

program
  .configureOutput({
    // Visibly override write routines as example!
    writeOut: (str) => process.stdout.write(`[OUT] ${str}`),
    writeErr: (str) => process.stdout.write(`[ERR] ${str}`),
    // Highlight errors in color.
    outputError: (str, write) => write(errorColor(str))
  });
```