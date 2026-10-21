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

## Development with VS Code Dev Containers

Open the component checkout in VS Code and reopen it in the Dev Container. The setup uses Docker Compose with a persistent cache volume shared by all builds.

On Windows, run the VS Code task **Build ESPHome test configs** to compile every `.yaml` and `.yml` file under `test/` with the ESPHome `stable`, `beta`, and `dev` images. The script detects the GitHub owner/repository from the checkout's `origin` remote and replaces matching ESPHome GitHub component sources with the local `components/` directory in temporary copies of the test files. The original test YAML files are not modified.

Run the separate VS Code task **Validate ESPHome component** for strict YAML linting, C/C++ formatting, Ruff format/lint, flake8, pylint, pyupgrade, and ESPHome-style text convention checks. Tool versions are pinned to the versions used by ESPHome CI, and the existing `.yamllint`, `.clang-format`, `.flake8`, and `pyproject.toml` configurations are used. clang-tidy is not included because it requires ESPHome's PlatformIO compilation metadata, which is not present in a standalone component checkout.

Use **Fix ESPHome component formatting/lint** to apply clang-format, Ruff formatting and safe Ruff lint fixes, pyupgrade, and portable text fixes, then rerun validation. `yamllint`, flake8, pylint, and semantic or unsupported lint findings are not automatically rewritten and will be reported for manual attention.

The build script can also be run directly with `-ConfigDirectory`, `-ComponentsDirectory`, and `-Versions` to select ESPHome tags, or with `-Image` to use one custom image. If the checkout does not have a GitHub `origin`, pass `-ComponentSource owner/repository` explicitly.
