# frothy-servo

A small [Frothy](https://frothy.dev) library for driving hobby servos over
PWM. Pure Frothy — no C to compile.

## Use it

Add it to your project's `frothy.toml`, pinned to a commit:

```toml
[deps]
servo = { git = "https://github.com/nikokozak/frothy-servo", rev = "<commit>" }
```

Then build and flash as usual:

```sh
frothy build
frothy flash --port /dev/cu.usbserial-0001
```

`frothy build` fetches the library, compiles its words into your firmware,
and they are available at the prompt.

## Words

| Word | Signature | What it does |
|------|-----------|--------------|
| `servo.attach` | `pin -> handle` | Open a 50 Hz PWM channel on `pin`. |
| `servo.write` | `handle, angle` | Move to `angle` degrees (0–180). |
| `servo.pulse` | `handle, us` | Drive a raw pulse width in microseconds. |
| `servo.detach` | `handle` | Release the pin; the servo stops holding. |

## Wire it

A servo has three leads: signal, power, and ground.

- Signal to a GPIO pin (say pin 25).
- Power to 5 V. A micro servo can run off the board's 5 V for light loads;
  anything with torque wants its own supply.
- Ground to the board's ground — shared ground is required.

```
> s is servo.attach: 25
ok
> servo.write: s, 90
ok
> servo.write: s, 0
ok
> servo.detach: s
ok
```

## Calibrate

If your servo does not reach both ends, or buzzes at the limits, its pulse
range is not the standard 500–2500 µs. Use `servo.pulse` to find the real
endpoints:

```
> servo.pulse: s, 1500   -- centre
> servo.pulse: s, 700    -- try each end until it just reaches, no buzz
> servo.pulse: s, 2300
```

## License

MIT.
