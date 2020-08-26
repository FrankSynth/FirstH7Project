# FirstH7Project

Ziel: simple ersten code compilen und uebertragen. Debugger testen.


Anleitung zum Compilen mit vscode:
https://marketplace.visualstudio.com/items?itemName=bmd.stm32-for-vscode#

Github Repo:
[Github stm32-for-vscode](https://github.com/bmd-studio/stm32-for-vscode)

All die Plugins muessen installiert und im PATH hinterlegt werden. STM Plugins und neue Software (CubeMX und ST Link): \
ownCloud\Synth\stm

Als Debugger **OpenOCD**.

## tasks.json:
Um das Projekt in cpp zu compilen reicht es, eine main.cpp anzulegen und den Inhalt der main.c rueber zu kopieren. Allerdings fehlt nach wie vor die cpp standard library im compile-Befehl.

Dieser Task compiled mit der cpp standard library
```
{
            "label": "Build STM cpp",
            "type": "shell",
            "command": "make -f STM32Make.make LIBS=\"-lc -lm -lnosys -lstdc++\"",
            "options": {
                "cwd": "${workspaceRoot}"
            },
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "problemMatcher": [
                "$gcc"
            ]
        },
```

Die Tasks koennen mit **ctrl+shift+b** aufgerufen werden. Da gibt auch den Flash-Befehl.

Die Tasks im **ctrl+shift+p** menue muss man evtl irgend wo anders updaten. Der **build and flash to an stm32 platform** Befehl compiled da nach wie vor nicht mit der cpp standard library.

## launch.json:
Damit beim debuggen zuvor ebenfalls mit cpp compiled wird, muss diese Zeile geaendert werden.
```
"preLaunchTask": "Build STM cpp",
```

## Neue Middleware
Aktivierte USB-Modes fuegen dem Projekt Middleware hinzu. Zum korrekten funktionieren von vscode muessen die includes in der **c_cpp_properties.json** ergaenzt werden.

```
"includePath": [
        "Drivers/CMSIS/Device/ST/STM32H7xx/Include",
        "Drivers/CMSIS/Include",
        "Drivers/STM32H7xx_HAL_Driver/Inc",
        "Drivers/STM32H7xx_HAL_Driver/Inc/Legacy",
        "Middlewares/ST/STM32_USB_Device_Library/Class/AUDIO/Inc",
        "Middlewares/ST/STM32_USB_Device_Library/Core/Inc",
        "Inc"
      ],
```

## weitere vscode include errors, z.B. math.h vermeiden
Ebenfalls in der **c_cpp_properties.json**:
```
"compilerPath": "arm-none-eabi-gcc",
```
zu folgendem aendern:

```
"compilerPath": "C:/Program Files (x86)/GNU Arm Embedded Toolchain/9 2020-q2-update/bin/arm-none-eabi-gcc.exe",
```

vscode checkt den Aufruf mit der PATH Variable nicht ganz, dadurch stimmt das code linting nicht, includes werden nicht alle erfasst. So gehts.

## offene Fragen

* weitere Libraries einfuegen (geht ueber den task Befehl, siehe [GitHub issue](https://github.com/bmd-studio/stm32-for-vscode/issues/29))
* Serial Monitor
* alles :D

Als alternative habe ich ebenfalls PlatformIO getestet, aber die verwendeten Treiber fuer den Chip sind dort leider viel zu alt und mit unserem Board nicht mehr kompatibel.