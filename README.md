# MIST - macOS Installer Super Tool

A Mac command-line tool that automatically downloads **macOS Installers** / **Firmwares**:

![Intel](Readme%20Resources/Intel.png)

![Apple](Readme%20Resources/Apple.png)

## Features

- [x] List all available macOS Installers / Firmwares available for download:
  - Display names, versions, builds, sizes and release dates
  - Additionally list beta versions of macOS in search results
  - Optionally export list as **CSV**, **JSON**, **Property List** or **YAML**
- [x] Download an available macOS Installer / Firmware:
  - For Intel based Macs:
    - Generate an Application Bundle (.app)
    - Generate a Disk Image (.dmg)
    - Generate a macOS Installer Package (.pkg)
      - Supports **macOS Big Sur** packages with a massive 12GB+ payload!
    - Optionally codesign Disk Images and macOS Installer Packages
    - Check for free space before attempting any downloads or installations
    - Optionally specify a custom catalog URL, allowing you to list and download macOS Installers from the following:
      - **Customer Seed** - AppleSeed Program
      - **Developer Seed** - Apple Developer Program
      - **Public Seed** - Apple Beta Software Program
  - For Apple Silicon Macs:
    - Download a Firmware Restore file (.ipsw)
    - Validates the SHA-1 checksum upon download

## Usage

```bash
OVERVIEW: macOS Installer Super Tool.

Automatically download macOS Installers / Firmwares.

USAGE: mist <subcommand>

OPTIONS:
  -h, --help              Show help information.

SUBCOMMANDS:
  list                    List all macOS Installers / Firmwares available to download.
  download                Download a macOS Installer / Firmware.
  version                 Display the version of mist.

  See 'mist help <subcommand>' for detailed help.
```

## Examples

```bash
# List all available macOS Firmwares for Apple Silicon Macs:
mist list --platform "apple"

# List all available macOS Installers for Intel Macs:
mist list --platform "intel"

# List all available macOS Installers for Intel Macs, including betas:
mist list --platform "intel" --include-betas

# List only macOS Big Sur Installers for Intel Macs:
mist list "macOS Big Sur" --platform "intel"

# List only the latest macOS Big Sur Installer for Intel Macs:
mist list "macOS Big Sur" --platform "intel" --latest

# List + Export macOS Installers to a CSV file:
mist list --export "/path/to/export.csv"

# List + Export macOS Installers to a JSON file:
mist list --export "/path/to/export.json"

# List + Export macOS Installers to a Property List:
mist list --export "/path/to/export.plist"

# List + Export macOS Installers to a YAML file:
mist list --export "/path/to/export.yaml"

# Download the latest macOS Big Sur Firmware for
# Apple Silicon Macs, with a custom name:
mist download "Big Sur" --platform "apple" --firmware-name "Install %NAME %VERSION%-%BUILD%.ipsw"

# Download the latest macOS Big Sur Installer for Intel Macs:
mist download "Big Sur" --platform "intel" --application

# Download a specific macOS Installer version for Intel Macs:
mist download "11.4" --application

# Download a specific macOS Installer version for
# Intel Macs, with a custom name:
mist download "11.4" --application --application-name "Install %NAME %VERSION%-%BUILD%.app"

# Download a specific macOS Installer version
# and generate a Disk Image with a custom name:
mist download "11.4" --image --image-name "Install %NAME %VERSION%-%BUILD%.dmg"

# Download a specific macOS Installer build and generate
# a codesigned Disk Image output to a custom directory:
mist download "19H15" \
     --image \
     --image-signing-identity "Developer ID Application: Name (Team ID)" \
     --output-directory "/path/to/custom/directory"

# Download the latest macOS Big Sur Installer and generate
# an Installer Application bundle, a Disk Image and
# macOS Installer Package, both with custom names, codesigned,
# output to a custom directory:
mist download "Big Sur" \
     --application \
     --application-name "Install %NAME% %VERSION%-%BUILD%.app" \
     --image \
     --image-name "Install %NAME% %VERSION%-%BUILD%.dmg" \
     --image-signing-identity "Developer ID Application: Name (Team ID)" \
     --package \
     --package-name "Install %NAME% %VERSION%-%BUILD%.pkg" \
     --package-identifier "com.mycompany.pkg.install-%NAME%" \
     --package-signing-identity "Developer ID Installer: Name (Team ID)" \
     --output-directory "/path/to/custom/directory"
```

## Build Requirements

- Swift **5.5**.
- Runs on macOS Yosemite **10.10** and later.

## Download

Grab the latest version of **Mist** from the [releases page](https://github.com/ninxsoft/MIST/releases).

## Credits / Thank You

- Project created and maintained by Nindi Gill ([ninxsoft](https://github.com/ninxsoft)).
- Apple ([apple](https://github.com/apple)) for [Swift Argument Parser](https://github.com/apple/swift-argument-parser), used to perform command line argument and flag operations.
- JP Simard ([jpsim](https://github.com/jpsim)) for [Yams](https://github.com/jpsim/Yams), used to export YAML.
- Callum Jones ([cj123](https://github.com/cj123)) for [IPSW Downloads API](https://ipswdownloads.docs.apiary.io), used to determine macOS Firmware metadata.

## Version History

- 1.6.1

  - `mist version` now correctly displays the current version when offline

- 1.6

  - SUCatalog URLs have been updated to point to **macOS Monterey (12)** URLs
  - Beta versions of macOS are now excluded by default in search results
    - Use `--include-betas` to include betas in search results
  - `mist version` now informs you if a new version is available for download
  - Bumped [Swift Argument Parser](https://github.com/apple/swift-argument-parser) version to **1.0.1**

- 1.5

  - Added List search support
    - `mist list <search-string>` to filter on results
    - `--latest` to filter the latest (first) match found
    - `--quiet` to suppress verbose output
    - `--output-type` to specify a custom output type
  - Added progress indicators
    - Displays current and total download amounts
    - Displays overal percentage downloaded
  - macOS Firmwares and Installers will fail to download if they already exist
    - Use `--force` to overwrite this behaviour
  - Faster macOS Firmwares list retrieval time
  - Faster macOS Installers list retrieval time
  - Replaced **SANITY CHECKS** headers with more inclusive **INPUT VALIDATION**
  - Fixed a bug with partial string matching when searching for downloads
  - Improved error handling and messaging

- 1.4

  - Support for downloading macOS Firmware (IPSW) files
    - Shasum is validated upon download
  - Moved list, download and version options to subcommands:
    - `mist --list` is now `mist list`
    - `mist --download` is now `mist download`
    - `mist --version` is now `mist version`
    - See `mist <subcommand> --help` for detailed help
  - Renamed `--list-export` option to `--export`
  - Re-added `--application` output option, back by popular demand!
  - Removed short flags for output options due to naming collisions:
    - Removed `-a` for `--application`
    - Removed `-i` for `--image`
    - Removed `-p` for `--package`
  - Lists now display / export total size
  - More verbose output for input validation

- 1.3.1

  - Fixed bug where SUCatalog files were not being parsed correctly

- 1.3

  - Removed `--name`, `--mac-os-version` and `--build` options, `--download` now supports all three
  - Removed `--list-format` option and renamed `--list-path` to `--list-export`, file extension determines export type
  - Removed `--application` and `--zip` options
  - Added `--catalogURL`
  - Added `--temporary-directory` option
  - Added `--keychain` option
  - Added free space check before downloads are initiated
  - Support for building hardware specific installers on all Macs
  - macOS name is now determined from the distribution files, no longer hardcoded
  - CSV cells with spaces now display correctly
  - Better input validation before downloads are initiated
  - Cleanup of standard output messaging (less verbose)
  - Removed download progress output
  - General code refactoring

- 1.2

  - Downloads now show progress: current + total download sizes and % completed
  - Mist will now create the `--output` directory if it does not exist

- 1.1.1

  - `--application` and `--zip` flags are now detected correctly

- 1.1

  - Specify custom catalog seeds: **Customer**, **Developer** and **Public**
    - This allows downloading macOS Install Betas
  - Output the macOS Installer application bundle to a custom directory
  - Generate a ZIP archive of the macOS Installer application bundle
  - Checks for free space before attempting any downloads and installations
  - Cleaned up CLI argument flags, options, and formatting

- 1.0
  - Initial release

## License

> Copyright © 2021 Nindi Gill
>
> Permission is hereby granted, free of charge, to any person obtaining a copy
> of this software and associated documentation files (the "Software"), to deal
> in the Software without restriction, including without limitation the rights
> to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
> copies of the Software, and to permit persons to whom the Software is
> furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all
> copies or substantial portions of the Software.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
> IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
> FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
> AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
> LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
> OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
> SOFTWARE.
