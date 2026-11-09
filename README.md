# ESPHome Component RGBWW POWER LIMITED

A modified RGBWW Light Component that will limit the maximum power the light can output. A good use case is a light strip that will get too hot if all channels of LED are on full brightness but not if only the Red, Green and Blue LEDs are on 100%. This light reduces the brightness when max power is set in the config and the max requested power will be too great. By reducing the brightness the color output will not change much if at all.

```yaml
# Sample configuration entry example
external_components:
  - source: github://mikelawrence/esphome-component-rgbww-power-limited

output:
  - platform: ledc
    id: output_red_int
    pin: GPIO40
  - platform: ledc
    id: output_green_int
    pin: GPIO39
  - platform: ledc
    id: output_blue_int
    pin: GPIO38
  - platform: ledc
    id: output_cw_int
    pin: GPIO37
  - platform: ledc
    id: output_ww_int
    pin: GPIO36

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

## Light

Configure the `rgbww_power_limited` light using five float outputs.

```yaml
light:
  - platform: rgbww_power_limited
    name: "Light"
    red: output_red_int
    green: output_green_int
    blue: output_blue_int
    cold_white: output_cw_int
    warm_white: output_ww_int
    cold_white_color_temperature: 6500 K
    warm_white_color_temperature: 2800 K
    max_power: 0.6
```

### Configuration Variables

* **output_id** (*Optional*, ID): ID of the light output; use this ID with `rgbww_power_limited.set_max_power`.
* **red**, **green**, **blue**, **cold_white**, **warm_white** (*Required*, output ID): IDs of the five float outputs that drive each LED channel.
* **cold_white_color_temperature**, **warm_white_color_temperature** (*Required*, color temperature): Color temperatures for the cold- and warm-white channels.
* **constant_brightness** (*Optional*, boolean): Limit the combined cold- and warm-white channels to the brightness of one channel. Defaults to `false`.
* **color_interlock** (*Optional*, boolean): Prevent RGB and white channels from being on at the same time. Defaults to `false`.
* **max_power** (*Optional*, float, default: `1.0`)
  Limits the total power. Range is 0.01 to 1.0 where 1.0 is 100% power and no power limiting will occur.

* **weight_red**, **weight_green**, **weight_blue**, **weight_cold_white**, **weight_warm_white** (*Optional*, positive float): Relative power weights for each channel. Each defaults to `1.0`.

* All other options from [Light](https://esphome.io/components/light/#config-light).

## Automations

### `rgbww_power_limited.set_max_power` Action

Changes the power limit at runtime.

#### Configuration Variables

* **id** (*Required*, ID): The light's `output_id`, not the light entity's `id`.
* **max_power** (*Required*, templatable float): The new limit, from `0.01` to `1.0`.

#### Configuration Examples

```yaml
esphome:
  on_boot:
    then:
      - rgbww_power_limited.set_max_power:
          id: rgbww_light_1_output
          max_power: 0.6
```

## Development with VS Code Dev Containers

Open the component folder in VS Code and select **Reopen in Container** for development. Docker Desktop is required. The tasks below run from the Windows host workspace using **Terminal > Run Task**:

- **ML ESPHome: Build ESPHome test configs**: The default build task. Validates first, then compiles the test configurations with ESPHome stable, beta, and dev using the local component source.
- **ML ESPHome: Validate ESPHome component**: Applies supported automatic fixes and checks the component without building test configurations.
- **ML ESPHome: Clear cached downloads**: Clears shared ESPHome, PlatformIO, and pip caches and removes downloaded ESPHome Docker images. Close the Dev Container before running it.

Use **Terminal > Run Build Task** to run the default build task. Test configuration builds serve as integration tests; there is no separate unit-test runner.
