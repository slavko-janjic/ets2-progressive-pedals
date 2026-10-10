# ETS2 / ATS Progressive Pedals

Progressive throttle and brake for **keyboard and mouse** players in Euro Truck Simulator 2, and in principle American Truck Simulator.

By default, pressing W or the left mouse button means 100% throttle, and S or the right mouse button means 100% brake. This config turns those keys into a pedal that you press gradually. You don't need mods, plugins or third-party tools. It's just your profile's `controls.sii`.

## How it feels

| Input | Behaviour |
|---|---|
| Hold **W** or **LMB** | Throttle builds gently: about 17% after 4 s, then speeds up to 100% around 6 s later |
| Tap **W** or **LMB** in a rhythm | Holds a steady part throttle. Longer presses push harder |
| Hold **S** or **RMB** | Brake builds up over about 4 s |
| Release | The pedal fades out instead of cutting instantly |
| **LMB + W** together | Emergency: instant full throttle |
| **RMB + S** together | Emergency: instant full brake |
| Brake key while on throttle | Throttle is cut immediately, and the reverse |

Tested on ETS2 **1.61** with a 25 t load. All timings assume about **60 FPS** (see [Tuning](#tuning)).

## Install

1. **Close the game.**
2. Find your profile folder:
   - Local profile: `Documents\Euro Truck Simulator 2\profiles\<hex id>\`
   - Steam Cloud profile: `Documents\Euro Truck Simulator 2\steam_profiles\<hex id>\`
   - Ignore folders with `.bak` in their name. Those are automatic backups.
3. **Back up** your `controls.sii` by renaming it, for example to `controls_backup.sii`.
4. Choose one:
   - **Easy:** copy [`example/controls.sii`](example/controls.sii) into the folder. Note that this replaces your other key bindings with mine (default keys and mouse steering).
   - **Keep your bindings:** follow [`PATCH.md`](PATCH.md).
5. Start the game. If Steam reports a cloud sync conflict, choose **local files**.

> ⚠️ Don't rebind throttle or brake in *Options → Controls*. The menu shows "Complex" for them, and changing them overwrites the ramp. Other keys are safe to change.

## Tuning

These constants are at the end of `controls.sii`. The values are per frame, so double them at 30 FPS and halve them at 120 FPS.

| Constant | Default | Meaning |
|---|---|---|
| `c_thr_up` | 0.0007 | How fast throttle builds (gentle phase) |
| `c_thr_fast` | 2.3 | Extra build rate after holding 4 s (0 = off) |
| `c_thr_dn` | 0.002 | How fast throttle fades after release |
| `c_thr_max` | 1.0 | Throttle cap (0.25 = never more than 25%) |
| `c_brk_up` | 0.004 | How fast the brake builds |
| `c_brk_fast` | 1.0 | Extra brake build rate after holding 0.7 s |
| `c_brk_dn` | 0.01 | How fast the brake releases |

The 4 s and 0.7 s hold thresholds are the `long_press(..., 4000)` and `long_press(..., 700)` values inside the `aforward` and `abackward` lines.

## How it works

ETS2's control expressions (SCS's [advanced input configuration](https://modding.scssoft.com/wiki/Documentation/Engine/Advanced_input_configuration)) let a `mix` reference its own previous value, which makes per-frame accumulation possible.

- The digital throttle and brake mixes (`dforward`, `dbackward`) are disabled, because the game treats them as all-or-nothing.
- The keys drive the **analog** pedal mixes (`aforward`, `abackward`) instead. They add a small step each frame while held and subtract a step when released.
- `long_press()` speeds up the ramp after a sustained hold.
- Two-key combos give the emergency 100%.
- Any real analog pedal (wheel or gamepad) keeps working, because the result is `max()`-ed with the original analog input.

## Known limitations

- Ramp speed depends on frame rate, because the game provides no time source in expressions.
- There's no on-screen pedal indicator, which would need a telemetry plugin.
- It hasn't been tested in ATS yet, but it uses the same engine and config format.

## License

MIT
