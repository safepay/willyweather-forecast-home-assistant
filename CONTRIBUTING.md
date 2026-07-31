# Contributing to the WillyWeather Integration

Thanks for your interest in contributing to the WillyWeather integration for
Home Assistant.

## Development Setup

1. Fork the repository
2. Clone your fork: `git clone https://github.com/yourusername/willyweather-forecast-home-assistant.git`
3. Create a branch: `git checkout -b fix/your-change`

To test locally, copy (or symlink) `custom_components/willyweather` into your
Home Assistant `config/custom_components/` directory and restart.

Enable debug logging while working on the coordinator or config flow:

```yaml
logger:
  default: warning
  logs:
    custom_components.willyweather: debug
```

## Directory Structure

```
willyweather-forecast-home-assistant/
├── custom_components/
│   └── willyweather/
│       ├── __init__.py          # Integration setup, device registration, entity cleanup
│       ├── manifest.json        # Integration metadata (version lives here)
│       ├── config_flow.py       # Config and options flow
│       ├── coordinator.py       # DataUpdateCoordinator and API calls
│       ├── const.py             # Constants and sensor type definitions
│       ├── weather.py           # Weather entity platform
│       ├── sensor.py            # Sensor platform
│       ├── binary_sensor.py     # Warning binary sensor platform
│       ├── strings.json         # UI strings
│       └── translations/
│           └── en.json          # English translations
├── .github/
│   ├── RELEASING.md             # How releases are cut
│   └── workflows/               # Hassfest, HACS validation, release
├── README.md
├── CHANGELOG.md
├── hacs.json                    # HACS metadata
└── info.md                      # Shown inside HACS
```

## Minimum Home Assistant Version

The integration targets **Home Assistant 2025.3.0 and newer**, declared in
[hacs.json](hacs.json). If a change requires a newer Home Assistant API, raise
that value in the same pull request and say so in the description — otherwise
users on older versions get an integration that fails to load rather than a
clear "requires a newer Home Assistant" message from HACS.

## Code Style

- Follow PEP 8
- Use type hints on function signatures
- Add docstrings to modules, classes and methods
- Prefer the Home Assistant helpers over rolling your own:
  - `async_get_clientsession(hass)` for HTTP — never construct a `ClientSession`
  - `asyncio.timeout` for timeouts (`async_timeout` is banned in Home Assistant)
  - `entry.runtime_data` for per-entry state, not `hass.data`

## Changing Sensors

Entity `unique_id`s are what tie an entity to its history. Changing one orphans
the old entity and starts the recorder history over, which users notice.

If a change renames or removes an entity:

- Say so explicitly in the pull request
- Add an entry to [CHANGELOG.md](CHANGELOG.md) describing what users must do
- Expect it to be released as a **major** version bump

## API Usage

The WillyWeather API is billed per call, so changes to polling behaviour carry a
real cost for users. If a change alters how often the integration calls the API,
or adds a new endpoint:

- State the before/after call volume in the pull request
- Put new endpoints behind a config option, defaulting to off, when they are not
  needed by every user

## Testing

There is no automated test suite yet, so please verify by hand and say what you
checked:

1. The integration loads without errors on a fresh install
2. The config flow completes, including auto-detection of the station
3. The options flow saves and reloads correctly
4. The entities you touched appear with the expected state and attributes
5. Reloading the entry (**Configure** → save) does not leave duplicate or
   orphaned entities
6. Nothing new appears in the log at WARNING or above

Coastal features (tides, swell) need a coastal station to test — if you cannot,
say so rather than assuming.

## Pull Requests

1. Update the README if you add or change a feature
2. **Do not bump the version** in `manifest.json` — the release workflow does
   that. See [.github/RELEASING.md](.github/RELEASING.md)
3. Give the PR a descriptive title; release notes are generated from PR titles,
   so the title becomes the changelog entry
4. Link any related issues

Hassfest and HACS validation run automatically on every pull request. Both need
to be green before merge.

## Reporting Issues

Please include:

- Home Assistant version
- Integration version (shown on the device page)
- Relevant log output with `custom_components.willyweather` set to debug
- Your station ID or location, and whether it is coastal
- Steps to reproduce, plus expected vs actual behaviour

Redact your API key from any logs before posting.

## Questions

Open an issue with your question or discussion topic.

Thank you for contributing!
