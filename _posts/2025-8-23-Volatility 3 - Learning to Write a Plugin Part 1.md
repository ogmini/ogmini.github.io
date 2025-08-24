---
layout: post
title: Volatility 3 - Learning to Write a Plugin Part 1
author: 'ogmini'
tags:
 - Volatility
---

In between prepping for my upcoming talk at BSides NYC, I've been slowly starting to learn how to write plugins for Volatility 3. I started with reading as much documentation and other writeups as possible on the process. These included:

- [https://volatility3.readthedocs.io/en/latest/simple-plugin.html](https://volatility3.readthedocs.io/en/latest/simple-plugin.html)
- [https://reversea.me/index.php/writing-a-volatility-3-plugin/](https://reversea.me/index.php/writing-a-volatility-3-plugin/)

For me at least, I wasn't able to find many other resources to learn how to write a Volatility 3 plugin. I'm also not a Python developer by trade so there is a small learning curve there. I had a bunch of missteps and mistakes in just trying to get a simple "test" plugin to run successfully. There is a lot to absord with how Volaility 3 works and operates. The technical documentation is definitly there. It is just a matter of digesting it all and putting it into practice.

My "test" plugin follows the documentation on Volatility 3's documentation. Oddly, I had to make some changes to get it to work and I admit some of that might be due to my own mistakes. Hopefully someone can point out my mistake. The script is below:

~~~ python
from typing import List

from volatility3.framework import renderers, interfaces
from volatility3.framework.configuration import requirements
from volatility3.framework.interfaces import plugins
from volatility3.framework.renderers import format_hints
from volatility3.plugins.windows import pslist
from volatility3.framework import exceptions

class notepad(plugins.PluginInterface):
    _required_framework_version = (2, 0, 0)

    @classmethod
    def get_requirements(cls):
        return [
            requirements.ModuleRequirement(
                name = 'kernel',
                description = 'Windows kernel',
                architectures = ["Intel32", "Intel64"]
            ),
            requirements.ListRequirement(
                name = 'pid',
                element_type = int,
                description = "Process IDs to include (all other processes are excluded)",
                optional = True
            ),  
        ]

    def run(self):

        filter_func = pslist.PsList.create_pid_filter(self.config.get('pid', None))

        return renderers.TreeGrid(
            [
                ("PID", int),
                ("Process", str),
                ("Base", format_hints.Hex),
                ("Size", format_hints.Hex),
                ("Name", str),
                ("Path", str),
            ],
            self._generator(
                pslist.PsList.list_processes(
                    context=self.context,
                    kernel_module_name=self.config['kernel'],
                    filter_func = filter_func
                )
            )
        )

    def _generator(self, procs):

        for proc in procs:

            for entry in proc.load_order_modules():

                BaseDllName = FullDllName = renderers.UnreadableValue()
                try:
                    BaseDllName = entry.BaseDllName.get_string()
                    # We assume that if the BaseDllName points to an invalid buffer, so will FullDllName
                    FullDllName = entry.FullDllName.get_string()
                except exceptions.InvalidAddressException:
                    pass

                yield (0, (proc.UniqueProcessId,
                        proc.ImageFileName.cast("string", max_length = proc.ImageFileName.vol.count,
                                                errors = 'replace'),
                        format_hints.Hex(entry.DllBase), format_hints.Hex(entry.SizeOfImage),
                        BaseDllName, FullDllName))
~~~

In the documentation they mention having a requirement for the version of pslist within the get_requirements. I ended up removing this to get the plugin to run.

~~~ python
requirements.VersionRequirement(
            name = 'pslist',
            component = pslist.PsList,
            version = (2, 0, 0)
        ),
~~~

Next steps are to actually get this script to do what I want. This is just the stereotypical "Hello World!" or should I saw "Hello Volatlity 3!".
