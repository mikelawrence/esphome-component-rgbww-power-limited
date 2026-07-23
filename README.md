# ESPHome Component RGBWW POWER LIMITED

A modified RGBWW Light Component that will limit the maximum power the light can output. A good use case is a light strip that will get too hot if all channels of LED are on full brightness but not if only the Red, Green and Blue LEDs are on 100%. This light reduces the brightness when max power is set in the config and the max requested power will be too great. By reducing the brightness the color output will not change much if at all.

```yaml
# Sample configuration entry example
external_components:
  - source: github://mikelawrence/esphome-components-rgbww-power-limited

light:
  - platform: rgbww_power_limited
    id: rgbww_light_1
    output_id: rgbww_light_1_output
    name: "Light"
    red: output_red_int
    green: output_green_int
    blue: output_blue_int
    cold_white: output_cw_int
    warm_white: output_ww_int
    cold_white_color_temperature: 6500 K
    warm_white_color_temperature: 2800 K
    constant_brightness: false
    color_interlock: false
    restore_mode: RESTORE_DEFAULT_OFF
    gamma_correct: 1.0
    # Power limiting, Cap at 60% of total system power for this light
    max_power: 0.6
```

## Configuration Variables

* **max_power** (*Optional*, float)
  Limits the total power. Range is 0.01 to 1.0 where 1.0 is 100% power and no power limiting will occur.

* All other options from [Light](https://esphome.io/components/light/#config-light).
