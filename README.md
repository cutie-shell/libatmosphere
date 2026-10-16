# libatmosphere

[![CircleCI](https://dl.circleci.com/status-badge/img/gh/cutie-shell/libatmosphere/tree/cutie.svg?style=svg)](https://dl.circleci.com/status-badge/redirect/gh/cutie-shell/libatmosphere/tree/cutie)

Configuration API for Atmospheres (themes) in Cutie. An atmosphere is a
wallpaper plus a colour palette.

## Building and installing

```
mkdir build
cd build
cmake ..
make
sudo make install
```

Requires Qt 6 (`Core`, `Quick`) and `cutiestore`.

## API

`AtmosphereModel` is a QObject intended to be used as a QML singleton
(`Atmosphere`).

### Properties

| Property | Type | Access | Notes |
|---|---|---|---|
| `path` | `QString` | read/write | Active atmosphere directory |
| `variant` | `QString` | read-only | `"dark"` or `"light"` |
| `primaryColor` | `QString` | read-only | `#rrggbb` |
| `primaryAlphaColor` | `QString` | read-only | `#aarrggbb` |
| `secondaryColor` | `QString` | read-only | `#rrggbb` |
| `secondaryAlphaColor` | `QString` | read-only | `#aarrggbb` |
| `accentColor` | `QString` | read-only | `#rrggbb` |
| `textColor` | `QString` | read-only | `#rrggbb` |
| `atmosphereList` | `QVariantList` | read-only | See below |

### Methods

| Method | Returns | Description |
|---|---|---|
| `setPath(QString path)` | `void` | Switch the active atmosphere |
| `extractPalette(QUrl wallpaperUrl)` | `QVariantMap` | Palette from a wallpaper; empty map if the image can't be loaded |
| `saveAtmosphere(QString name, QUrl wallpaperUrl, QVariantMap colors)` | `bool` | Create a user atmosphere; `false` on invalid name, unreadable wallpaper, existing name or write failure |
| `deleteAtmosphere(QString name)` | `bool` | Delete a user-created (`editable`) atmosphere; `false` otherwise |
| `themeSound(QString name)` | `QUrl` | Sound file from the current sound theme; empty `QUrl` if not found |

### Return value shapes

**`extractPalette` / `saveAtmosphere` colors** (`QVariantMap`, all values `QString`):
`primaryColor`, `primaryAlphaColor`, `secondaryColor`, `secondaryAlphaColor`,
`accentColor`, `textColor`, `variant`.

**`atmosphereList` entry** (`QVariantMap`):

| Key | Type |
|---|---|
| `name` | `QString` |
| `path` | `QString` |
| `variant` | `QString` |
| `editable` | `bool` |

### Signals

`pathChanged()`, `variantChanged()`, `primaryColorChanged()`,
`primaryAlphaColorChanged()`, `secondaryColorChanged()`,
`secondaryAlphaColorChanged()`, `accentColorChanged()`, `textColorChanged()`,
`atmosphereListChanged()`

## Atmosphere format

An atmosphere is a directory containing `wallpaper.jpg` and `settings.ini`
(colour values without `#`):

```ini
variant=dark
primaryColor=3a5f7d
primaryAlphaColor=803a5f7d
secondaryColor=8aa1b4
secondaryAlphaColor=658aa1b4
accentColor=d98c3f
textColor=ffffff
editable=true
```

User atmospheres are saved to `~/.local/share/atmospheres/<name>/`.