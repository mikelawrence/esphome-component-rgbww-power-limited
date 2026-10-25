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

* **red**, **green**, **blue**, **cold_white**, **warm_white** (*Required*, output ID): IDs of the five float outputs that drive each LED channel.
* **cold_white_color_temperature**, **warm_white_color_temperature** (*Required*, color temperature): Color temperatures for the cold- and warm-white channels.
* **max_power** (*Optional*, float)
  Limits the total power. Range is 0.01 to 1.0 where 1.0 is 100% power and no power limiting will occur.

* **weight_red**, **weight_green**, **weight_blue**, **weight_cold_white**, **weight_warm_white** (*Optional*, positive float): Relative power weights for each channel. Each defaults to `1.0`.

* All other options from [Light](https://esphome.io/components/light/#config-light).

## Actions

The `rgbww_power_limited.set_max_power` action changes the power limit at runtime. `max_power` can be a value from `0.01` to `1.0` or a templatable value.

```yaml
on_boot:
  then:
    - rgbww_power_limited.set_max_power:
        id: rgbww_light_1
        max_power: 0.6
```

## Development with VS Code Dev Containers

Open this component directory as the VS Code workspace and reopen it in the Dev Container. All component checkouts share the same persistent ESPHome, PlatformIO, and pip cache volumes; Docker also reuses locally downloaded ESPHome images.

Run the VS Code task **Clear cached downloads** to empty the shared cache volumes and remove locally downloaded `ghcr.io/esphome/esphome` images. Close the Dev Container before running the task so Docker can remove images that are in use.

On Windows, the VS Code **Terminal > Run Build Task** command runs the default **ML ESPHome: Build ESPHome test configs** task. It runs **ML ESPHome: Validate ESPHome component** first, and only compiles if validation succeeds. Validation applies supported automatic fixes, then runs strict YAML linting, C/C++ formatting checks, Ruff format/lint, flake8, pylint, pyupgrade, and ESPHome-style text convention checks. Compilation covers every `.yaml` and `.yml` file under `test/` with the ESPHome `stable`, `beta`, and `dev` images.

Tool versions are pinned to the versions used by ESPHome CI, and the existing `.yamllint`, `.clang-format`, `.flake8`, and `pyproject.toml` configurations are used. clang-tidy is not included because it requires ESPHome's PlatformIO compilation metadata, which is not present in a standalone component checkout.

The build stage detects the GitHub owner/repository from the checkout's `origin` remote and replaces matching ESPHome GitHub component sources with the local `components/` directory in temporary copies of the test files; original test YAML files are not modified. Pass `-ComponentSource owner/repository` to `.devcontainer\build.ps1` if the checkout has no GitHub `origin`; use `-Versions @("dev")` to select ESPHome tags, or `-Image ghcr.io/esphome/esphome:<tag>` to use one custom image.

Run `.devcontainer\validate.ps1` alone to apply fixes and run validation without compiling. `yamllint`, flake8, pylint, and semantic or unsupported lint findings are not automatically rewritten and will be reported for manual attention. There is no separate unit-test runner; compiling the test configs is the integration/build test.
