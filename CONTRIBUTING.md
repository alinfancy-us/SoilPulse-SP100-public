# Contributing

Thanks for taking the time to improve these docs. This repository is a usage reference for the SP100 sensor, not its firmware or dashboard source.

## What lives here

- `README.md` and `docs/`: device overview, calibration, GATT interface and troubleshooting
- `examples/home-assistant/`: automation examples you can adapt

## What does not live here

The firmware and the Web Bluetooth dashboard are not published in this repository. The sensor runs its firmware locally and needs no cloud account, and the protocol documented here is the one the device uses. Contributions here cover documentation and integration examples, not changes to the device software.

## What we welcome

- Corrections to facts, commands, UUIDs or byte formats in the docs
- Clearer wording, missing steps and fixes to broken links or images
- Additional Home Assistant automations and examples for other integration tools
- Translations, if you want to maintain one consistently

## What to put in an issue

Issues in this repository cover two things:

- **Documentation problems.** A wrong value, a stale screenshot, a step that does not work.
- **Integration questions.** Home Assistant setup, BTHome discovery, GATT reads and writes, or using the automations.

Before opening one, check [docs/troubleshooting.md](docs/troubleshooting.md) and the existing issues. Include your sensor's firmware version, the platform and browser or Home Assistant version, and the exact step where things stopped working. For a documentation fix, point to the file and line.

We read every issue and fold recurring answers back into the docs. We do not commit to a reply time. For questions about the device itself, email <support@alinfancy.com>.

## What not to put in an issue

- **Hardware failure or a dead sensor.** A device that will not power on, will not charge, has a broken probe or arrived damaged is a unit-level problem. Contact the retailer you bought it from, or email <support@alinfancy.com>. Issues here are public and cannot replace a warranty claim.
- **Private information.** Do not post serial numbers, your address, account details or credentials.
- **Feature requests for the firmware or dashboard.** There is no source here to change. You can still describe the use case as an integration need.

## Pull requests

Small fixes can go straight to a pull request. For a larger rewrite, open an issue first so we can agree on the scope. Keep changes focused, and describe what you changed and why. Markdown and YAML only, please.
