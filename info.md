# WillyWeather Integration

[![HACS](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/integration)
[![GitHub Release](https://img.shields.io/github/v/release/safepay/willyweather-forecast-home-assistant)](https://github.com/safepay/willyweather-forecast-home-assistant/releases)
[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-2025.3.0+-blue.svg)](https://www.home-assistant.io/)
![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)

A custom Home Assistant integration providing comprehensive weather data from WillyWeather Australia.

This differs from the BoM integration by providing separate binary sensors for warnings as well as data that is unavailable such as tide and swell information.

## Features

- **Weather Entity**: Real-time weather conditions with full daily and hourly forecast support
- **Observational Sensors**: Current weather measurements including temperature, humidity, pressure, wind, rainfall, and more
- **Sun/Moon Data**: Sunrise, sunset, moonrise, moonset, and moon phase information with dynamic moon phase icons
- **Tide Information**: High and low tide times and heights (Coastal locations. Also available in daily forecast data)
- **Swell Information**: Wave heights, periods and directions (Coastal locations. Also available in hourly forecast data)
- **Weather Warnings**: Binary sensors for active storm, flood, fire, heat, wind, frost warnings and more
- **Automatic Station Detection**: Automatically finds the closest WillyWeather station based on your Home Assistant location
- **Configurable Data**: Enable/disable optional sensors through the UI
- **Forecast Sensors**: Optional per-day sensors (0-6), preconfigured for the Platinum Weather Card
- **Configurable Update Intervals**: Separate day/night frequencies for observational and forecast data to manage API usage
- **Forecast Data**: Daily (7 days) and hourly (3 days) with comprehensive data points

## API Usage Management

The WillyWeather API is paid (roughly $1.20/month for a typical configuration), so
observational and forecast data are polled on **separate** schedules — current
conditions stay fresh while forecasts, which change far less often, are fetched
less frequently and cached in between.

- Observational defaults: 10 minutes during the day, 30 minutes at night
- Forecast defaults: 30 minutes during the day, 60 minutes at night
- Automatically switches between day and night modes
- 1-2 API calls per observational update, 2-3 when a forecast refresh is due
- Typical usage with defaults: ~7,900 calls/month

Configure update intervals during setup or anytime through **Settings** → **Devices & Services** → **WillyWeather** → **Configure**.

## Requirements

- Home Assistant 2025.3.0 or newer
- A [WillyWeather API key](https://www.willyweather.com.au/info/api.html)
