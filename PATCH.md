# Manual patch

Use this if you want to keep your own key bindings. You change 4 lines and add 7.

**Close the game first, and back up your `controls.sii`.**

## 1. Replace these 4 lines

Search for each `mix` name and replace the whole value between the quotes. Keep the `config_lines[N]:` part your file already has, because the numbers differ between profiles.

```
"mix dforward `0`"
"mix dbackward `0`"
"mix aforward `max(memory(j_throttle_c?1, normalize(sel(c_jzthrottle, sel(c_jithrottle, -j_throttle?0, j_throttle?0), sel(c_jithrottle, j_throttle?1 * -0.5 + 0.5, j_throttle?-1 * 0.5 + 0.5)), c_throt_dz)) + semantical.aforward?0, clamp(aforward + (bool((mouse.button_left?0 || keyboard.w?0)) * c_thr_up * (1 + (c_thr_fast * long_press((mouse.button_left?0 || keyboard.w?0), 4000)))) - ((1 - bool((mouse.button_left?0 || keyboard.w?0))) * c_thr_dn), 0, c_thr_max) * (1 - bool((mouse.button_right?0 || keyboard.s?0))), (mouse.button_left?0 & keyboard.w?0) * (1 - bool((mouse.button_right?0 || keyboard.s?0))))`"
"mix abackward `max(memory(j_brake_c?1, normalize(sel(c_jzbrake, sel(c_jibrake, -j_brake?0, j_brake?0), sel(c_jibrake, j_brake?1 * -0.5 + 0.5, j_brake?-1 * 0.5 + 0.5)), c_brake_dz)) + semantical.abackward?0, clamp(abackward + (bool((mouse.button_right?0 || keyboard.s?0)) * c_brk_up * (1 + (c_brk_fast * long_press((mouse.button_right?0 || keyboard.s?0), 700)))) - ((1 - bool((mouse.button_right?0 || keyboard.s?0))) * c_brk_dn), 0, 1) * (1 - bool((mouse.button_left?0 || keyboard.w?0))), (mouse.button_right?0 & keyboard.s?0) * (1 - bool((mouse.button_left?0 || keyboard.w?0))))`"
```

## 2. Add the 7 constants at the end of the list

Add them after the last `config_lines[...]` entry, continuing the numbering. For example, if your last line is `config_lines[600]`, the first new one is `config_lines[601]`.

```
config_lines[N+1]: "constant c_thr_up 0.000700"
config_lines[N+2]: "constant c_thr_dn 0.002000"
config_lines[N+3]: "constant c_thr_fast 2.300000"
config_lines[N+4]: "constant c_brk_up 0.004000"
config_lines[N+5]: "constant c_brk_dn 0.010000"
config_lines[N+6]: "constant c_brk_fast 1.000000"
config_lines[N+7]: "constant c_thr_max 1.000000"
```

## 3. Update the line count

Near the top of the file, add 7 to `config_lines: <number>`. For example, `601` becomes `608`.

If the count or numbering is wrong, the game will reject the file and reset your controls. Restore your backup and check the numbers again.

## Using different keys?

The `aforward` and `abackward` lines have **W, S, left mouse button and right mouse button** written into them. If you drive with other keys, swap the key names in **both** lines.

Each line contains these blocks, which may appear more than once:

| What | Appears as | Used for |
|---|---|---|
| Throttle keys | `(mouse.button_left?0 \|\| keyboard.w?0)` | Normal gradual throttle |
| Brake keys | `(mouse.button_right?0 \|\| keyboard.s?0)` | Normal gradual brake |
| Emergency throttle | `(mouse.button_left?0 & keyboard.w?0)` | Both keys together = 100% |
| Emergency brake | `(mouse.button_right?0 & keyboard.s?0)` | Both keys together = 100% |

Use find and replace in your text editor, and replace **every** occurrence. The throttle keys also appear inside the brake line, because pressing throttle cancels the brake, and the reverse.

### Common key names

| Key | Name to use |
|---|---|
| Letters | `keyboard.w`, `keyboard.s`, `keyboard.z`, `keyboard.x`, … |
| Arrow keys | `keyboard.uarrow`, `keyboard.darrow`, `keyboard.larrow`, `keyboard.rarrow` |
| Numpad | `keyboard.num8`, `keyboard.num5`, … |
| Space / Shift / Ctrl | `keyboard.space`, `keyboard.lshift`, `keyboard.lctrl` |
| Mouse buttons | `mouse.button_left`, `mouse.button_right`, `mouse.button_middle`, `mouse.button_4`, `mouse.button_5` |

Keep the `?0` after each name, for example `keyboard.uarrow?0`. It stops the line from failing if the key doesn't exist.

**Not sure of a key's name?** Bind it to any unused action in the game menu, then close the game and look in `controls.sii` for the name the game wrote. Change that binding back afterwards.

### Examples

Arrow keys only, with no mouse pedals:

- Throttle: `(keyboard.uarrow?0)`
- Brake: `(keyboard.darrow?0)`
- Emergency: there's no second key to pair with, so replace the emergency blocks with `0` to turn them off, or pair the arrows with another key such as `(keyboard.uarrow?0 & keyboard.rshift?0)`.

Keyboard only (W/S), with no mouse:

- Throttle: `(keyboard.w?0)`
- Brake: `(keyboard.s?0)`
- Emergency: replace with `0`, or pair with another key such as `(keyboard.w?0 & keyboard.lshift?0)`. Shift is the default gear-up key, so pick one you don't use.
