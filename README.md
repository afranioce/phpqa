# PHP QA Tools For Vim

This is a plugin for Vim that integrates PHP quality checking tools, to allow you to code to a particular standard and easily spot errors and violations.

It uses PHP linter to check for syntax errors, and integrates with [PHP Code Sniffer][1] and [PHP Mess Detector][2] to check for coding standard violations, and shows code coverage from clover XML files.

### Quick Guide

The plugin is configured by default to automatically run the QA tools when a PHP file is saved. Therefore, save a file and the linter will run. If there is a syntax error, the offending line will be highlighted. Plus, a [quickfix][3] window opens to show the error and it's position in the file.

If there are no syntax errors, PHP Code Sniffer and Mess Detector will run. These will require some configuration to fit your needs, which you can read about under the "Configuration" heading. The output of these two commands are combined, and the file is highlighted with the occurences. Again, a quickfix window opens, showing the violations and allowing you to navigate through them.

You can toggle markers with the following commands (in command mode):

```vim
<Leader>qa	" Show/hide code sniffer and mess detector violations
<Leader>qc	" Show/hide code coverage markers
```

What's the `<Leader>` key? It's likely to be either `\` or `,`, but you can set it from the command line or in your *.vimrc* file using:

```vim
let mapleader="@"
```

or whatever you want it to be.

You can also run each command separately on demand:

- `:Php` - check for syntax errors
- `:Phpcs` - run code sniffer
- `:Phpmd` - run mess detector (will ask for a rule XML file if not set) 
- `:Phpcc` - show code coverage (will ask for a clover XML file if not set)

If you generate clover code coverage reports with your tests, you can toggle markers to show which lines are covered and which aren't. You can run the command once using `Phpcs` as shown above, or you can configure it to load the markers every time you open a new file - see the configuration section for more information.

### Installation

Installation is easy-peasy if you're using [Vundle][4]. Just add this to your *.vimrc* file:

```vim
Bundle 'joonty/vim-phpqa.git'
```
and run `vim +BundleInstall +qall` from a terminal.

If you aren't using vundle, you will have to extract the files in each folder to the correct folder in *.vim/*.

**Note:** your vim installation must be compiled with *signs* and *perl* for this plugin to work.

### Configuration

Each command has it's own configuration settings, which allow you to get the functionality you want.

PHP mess detecotr needs a ruleset XML file (see the [mess detector website][2] for more information) to run, which you will be prompted for the first time the command runs. However, it's much easier to just specify it in your *.vimrc* file:

```vim
let g:phpqa_messdetector_ruleset = "/path/to/phpmd.xml"
```

For PHP code sniffer, you can pass arguments to the command line binary (run `phpcs --help` to see a list). For example:

```vim
" Set the codesniffer args (default = "--standard=PHPCS")
let g:phpqa_codesniffer_args = "--standard=Zend"
```

However, **don't** set the `--report=` argument, as it won't work!

For all the commands, you can override the executable:

```vim
" PHP executable (default = "php")
let g:phpqa_php_cmd='/path/to/php'

" PHP Code Sniffer binary (default = "phpcs")
let g:phpqa_codesniffer_cmd='/path/to/phpcs'

" PHP Mess Detector binary (default = "phpmd")
let g:phpqa_messdetector_cmd='/path/to/phpmd'
```
However, you don't need to do this if the commands `php`, `phpcs` and `phpmd` are can be found in your `$PATH` environment variable.

And you can stop them running automatically:

```vim
" Don't run messdetector on save (default = 1)
let g:phpqa_messdetector_autorun = 0

" Don't run codesniffer on save (default = 1)
let g:phpqa_codesniffer_autorun = 0

" Show code coverage on load (default = 0)
let g:phpqa_codecoverage_autorun = 1
```

For code coverage, you can specify a clover XML file to stop the prompt:

```vim
let g:phpqa_codecoverage_file = "/path/to/clover.xml"
```

### Acknowlegements

This plugin has reused and modified a lot of the code from the Vim [QuickHigh plugin][5], written by Brian Medley. It required too much modification to be able to use it as a stand-alone plugin, so it has been added to this code. My thanks goes to Brian for the work that's gone into that script.

### License

This plugin is released under the [GPL license][6].

QuickHigh was also released under the GPL license.


[1]: http://pear.php.net/package/PHP_CodeSniffer/redirected
[2]: http://phpmd.org/
[3]: http://vimdoc.sourceforge.net/htmldoc/quickfix.html
[4]: https://github.com/gmarik/vundle
[5]: http://www.vim.org/scripts/script.php?script_id=124
[6]: https://raw.github.com/joonty/vim-phpqa/master/LICENSE
