# Content
1. [Install Venus Extenstion](#1-install-venus-extension)
2. [Edit settings.json](#2-edit-settingsjson)  
    2.1. [launch configuration](#21-launch-configuration)  
        2.1.1 [No Existing Configurations](#211-no-existing-configurations)  
        2.1.2  [Existing Configurations](#212-existing-configurations)  
    2.2  [RISC-V association with run](#22-risc-v-association-with-run)
3. [Done!](#3-done)

## 1. Install Venus Extension
Go to the extensions tab in VS Code and search for "Venus" or "RISC-V Venus Simulator" and install the extension by hm. ( [Venus](https://marketplace.visualstudio.com/items?itemName=hm.riscv-venus "https://marketplace.visualstudio.com/items?itemName=hm.riscv-venus") )


## 2. Edit setting.json
Open the settings.json file by pressing `Ctrl + Shift + P` and typing "Preferences: Open User Settings (JSON)". 

### 2.1 Launch configuration

Press ctrl + f and search for "configurations". If you do not find the keyword 'configurations', follow the steps in [2.1.1 No Existing Configurations](#211-no-existing-configurations). If you do find the keyword 'configurations', follow the steps in [2.1.2 Existing Configurations](#212-existing-configurations).
#### 2.1.1 No Existing Configurations
Add the following code snippet anywhere in the files. Lets assume we are adding it at the very start of the file, after the opening curly brace `{` and inserting an empty line(for appearance).

      
    "launch": {
        "configurations": [
            {
                "type": "venus",
                "request": "launch",
                "name": "RISC-V",
                "program": "${file}",
                "stopOnEntry": false,
                "stopAtBreakpoints": true,
                "openViews": [
                    "Memory"
                    // "Robot",
                    // "LED Matrix",
                    // "Seven Segment Board"
                // ],
                ]
                // "ledMatrixSize": {
                //     "x": 10,
                //     "y": 10
                // }
            }
        ]
    },

#### 2.1.2 Existing Configurations
After locating the code snippet that shows 

    "launch": {
        "configurations": [
            {
                // existing configuration 1
            }
            .....
            {
                // existing configuration n
            }
        ]
    },

Go and insert the code snippet for venus configuration right after the beginning square brackets `[`
et that shows 

    "launch": {
        "configurations": [
            {
                "type": "venus",
                "request": "launch",
                "name": "RISC-V",
                "program": "${file}",
                "stopOnEntry": false,
                "stopAtBreakpoints": true,
                "openViews": [
                    "Memory"
                    // "Robot",
                    // "LED Matrix",
                    // "Seven Segment Board"
                // ],
                ]
                // "ledMatrixSize": {
                //     "x": 10,
                //     "y": 10
                // }
            },
            {
                // existing configuration 1
            }
            .....
            {
                // existing configuration n
            }
        ]
    },


### 2.2 RISC-V association with run
(Might just be info that i *think* is right, does not mean it necessarily is right).
To allow just normal runs after pressing the run button on the top right corner of VS Code when viewing a file (.s, assembly). Search for "code-runner.executorMap" in the settings.json file.
Modify the section that originally might look like this:

    "code-runner.executorMap": {
        "1st_language": "1st some command combination",
        "2nd_language": "2nd some command combination",
        ....
        "Nth language": "Nth some command combination"
    }

to include riscv like this:

    "code-runner.executorMap": {
        "riscv": "venus ${file}",
        "1st_language": "1st some command combination",
        "2nd_language": "2nd some command combination",
        ....
        "Nth language": "Nth some command combination"
    }

## 3. Done!
You are now all set(probably, hopefully) to run RISC-V assembly code using Venus in VS Code!

