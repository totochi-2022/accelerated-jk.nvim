# accelerated-jk.nvim (Visual Mode Fork)
This plugin accelerates j/k mappings' steps while j or k key is repeating. The original version is [rhysd/accelerated-jk](https://github.com/rhysd/accelerated-jk) and now i rewrited it with Lua.

**This is a fork that adds Visual mode support and simplifies key mapping configuration.**

## Changes from Original
- **Visual mode support**: All motions now work in visual mode (v, V, Ctrl-V)
- **Simplified configuration**: No need for `<Plug>` mappings - just add keys to `acceleration_motions`
- **New option**: `acceleration_motions_visual` for visual-mode specific motions

## Requirements
- Neovim latest stable version or nightly

## Installation
You can install `accelerated-jk.nvim` with your favorite package manager.

vim-plug:
```lua
Plug 'rainbowhxch/accelerated-jk.nvim'
```
packer.nvim:
```lua
use { 'rainbowhxch/accelerated-jk.nvim' }
```

## Usage
Simply add the keys you want to accelerate to `acceleration_motions` (for normal mode) and `acceleration_motions_visual` (for visual mode):

```lua
require('accelerated-jk').setup({
    acceleration_motions = { 'j', 'k', 'w', 'b', 'e' },         -- Normal mode
    acceleration_motions_visual = { 'j', 'k', 'w', 'b', 'e' },  -- Visual mode
})
```

**Note**: `<Plug>` mappings are still created for backward compatibility but are no longer necessary.


## Modes
All modes acceleration step according to the setting `acceleration_table`.

- `time_driven`: The default one. With this mode, if the interval of key-repeat takes more than `acceleration_limit` ms, the step is reset. If you want to decelerate up/down moving by time instead of reset, set `enable_deceleration` to `true`. In addition, if you want to change deceleration rate, set `deceleration_table` to a proper value. Even better, you can accelerate other movements (such as `w`, `b`) through `acceleration_motions`.

- `position_driven`: Reset steps using only position determination. Not sensitive enough, the effect is not good as the `time_driven` mode. Note that `acceleration_motions` isn't supported by this mode.

## Configuration
If you stasify the default configuration, nothing is need to changed. Otherwise you can call `require('accelerated-jk').setup()` to change the behavior of acceleration. The default configuration is:
```lua
require('accelerated-jk').setup({
    mode = 'time_driven',
    enable_deceleration = false,
    acceleration_motions = {},          -- Normal mode motions
    acceleration_motions_visual = {},   -- Visual mode motions
    acceleration_limit = 150,
    acceleration_table = { 7,12,17,21,24,26,28,30 },
    -- when 'enable_deceleration = true', 'deceleration_table = { {200, 3}, {300, 7}, {450, 11}, {600, 15}, {750, 21}, {900, 9999} }'
    deceleration_table = { {150, 9999} }
})
```

All configuration is here:

| Item                  | Value                              | Meaning                                                                                                                                                                                          |
|----------------       | ---------------                    | ---------------                                                                                                                                                                                  |
| `mode`                | 'time_driven' or 'position_driven' | Acceleration modes                                                                                                                                                                                |
| `enable_deceleration` | Boolean                            | Whether to enable deceleration                                                                                                                                                                   |
| `acceleration_motions` | List                              | Accelerated motions for normal mode, such as {'j', 'k', 'w', 'b'}.
| `acceleration_motions_visual` | List                      | Accelerated motions for visual mode, such as {'j', 'k', 'w', 'b'}.                                                                                                                                                     |
| `acceleration_limit`  | Integer                            | The accelerated limit for `time_driven` mode                                                                                                                                                     |
| `acceleration_table`  | List                               | Indexs represent steps of j/k mappings, values represent required number of typing j/k to advance steps. In the case of {5, 15, 29}, if j is hited: 1)less than 5 times, the acceleration step is 1, 2)more than 5 times and less than 15 times, the acceleration steps is 2, 3)more than 15 times and less than 29 times, the acceleration steps is 3, 4)and after 29 j hits, the acceleration steps is 4. |
| `deceleration_table`  | List                               | Every element is a pair which the first element is elapsed time after last j/k typed and the second element is the count to decelerate steps. In the case of {{200, 3}, {300, 7}}, if the elapsed time: 1)less than 200ms, the deceleration step is 1, 2)more than 200ms and less than 300ms, the deceleration steps is 3, 3)more than 300ms, then deceleration steps is 7. |

## Copyright
This plugin is distributed under MIT License.

    Copyright (c) 2022 rainbowhxch

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
    of the Software, and to permit persons to whom the Software is furnished to do so,
    subject to the following conditions:

    The above copyright notice and this permission notice shall be included in all
    copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
    INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR
    PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
    LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
    TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR
    THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## Fork Information
Original project: [rainbowhxch/accelerated-jk.nvim](https://github.com/rainbowhxch/accelerated-jk.nvim)  
Fork: [totochi-2022/accelerated-jk.nvim](https://github.com/totochi-2022/accelerated-jk.nvim)  
Fork created: 2025-08-22  
Modified for personal use with visual mode support
