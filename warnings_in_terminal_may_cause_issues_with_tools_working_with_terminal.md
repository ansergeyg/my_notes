Few yers ago I configured xdebug to have a newer version (v3). It worked but with some caveats.
It was throwing warning messages in the termninal every time you run a command from within the project workspace in that terminal.
It didn't affect any tool and program until one day I wanted to configure functional/kernal test using drupal + phpunit.
It turned out xdebug was affecting the execution of the tests somehow. For the sake of clean testing I should have disabled xdebug and check if
tests still run. That would help me a lot, but I only figured it out few days later.
