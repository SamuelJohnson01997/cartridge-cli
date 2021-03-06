# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- `--force` option for `start` command to send SIGKILL to instances

## [2.1.0] - 2020-07-17

### Added

- `cartridge log` command to get logs of instances running in background
- `--timeout` option for `start` command
- `--version, -v` command to print `cartridge-cli` version

## [2.0.1] - 2020-07-07

### Fixed

- Error on packing application without build hooks
- Unexpected end of JSON input error on image build

## [2.0.0] - 2020-07-02

### Changed
  
- Completely rewritten in Go
- `CARTRIDGE_BUILDDIR` is renamed to `CARTRIDGE_TEMPDIR`;
  now it can be project subdirectory
- `centos:7` is allowed to be used as a base image al well as `centos:8`
- `--tag` option for `pack` command is an array of strings now
- `start`, `stop`, `status` commands requires only instance names,
  application name is discovered from a rockspec or passed by `--name` option
- `cartridge.{pre,post}-build` hooks should be executable
- `cartridge-cli` can't be installed as a rock module
- build requires rockspec in the application directory


### Added

- `--quiet` flag to hide build output
- `--verbose` flag to make output more verbose
- `--data-dir` and `--log-dir` options for `start` command
- `--cache-from` and `--co-cache` options for `pack` command on building in docker
- `--stateboard-unit-template` option for `pack` command

### Removed

- `TARANTOOL_DOCKER_BUILD_ARGS` env variable for `pack` command
- deprecated `.cartridge.pre` and `.cartridge.ignore` support

## [1.8.3] - 2020-06-29

### Added

- New metrics role in template project

### Changed

- Update `cartridge` to 2.2.0

## [1.8.2] - 2020-05-28

### Fixed

- Project version isn't detected by git when `--tag` option is specified
- Fixed a bug in normalizing the version passed with `--version`.
  The expected pattern is `major.minor.patch[-count][-commit]`,
  but previously the normalization failed: `--version xxx1.2.3xxx`
  resolved to `1.2.3` instead an error.

## [1.8.1] - 2020-05-06

### Fixed

- Fixed docker image fullname

## [1.8.0] - 2020-04-27

### Added

- Cartridge Stateboard support:
  * Application template contains stateboard entrypoint script and configuration
  * Unit file for stateboard `systemd` service is delivered in RPM/DEB
  * Added `--stateboard` and `--stateboard-only` options for `start` and `stop`
    commands to start/stop stateboard locally
- Warning on running `cartridge start` without `cartridge build` before
- Checking notify socket length on `cartridge start -d`
- `cartridge status` command to check instances status

### Changed

- Prettified `start` and `stop` logs
- `start` and `stop` commands try to start/stop all instances and accumulate
  errors
- If instance is already stopped, `stop` command doesn't fail, only warning
  message is shown
- Update `cartridge` to 2.1.2

### Fixed

- `Not enough memory` error on running `cartridge pack`
- Error on project files and build directory containing symbols that
  should be escaped

## [1.7.0] - 2020-04-10

### Added

- Option `--suffix` for `pack` command to specify the result artifact name suffix.

## [1.6.0] - 2020-04-03

### Added

- Packing in docker. Added a new option `--use-docker` for `cartridge pack` command.
  This option allows to build application in docker image.

## [1.5.0] - 2020-03-27

### Changed

- Git errors aren't fatal, if `git clean` command fails (in the project root or
  for sumbodules), it just prints warning message
- Project build: one Lua-executable is compiled

## [1.4.2] - 2020-03-17

### Added

- Option `--build-from` to specify build image base layers.
- Add `TARANTOOL_DIR` to rockspec build.variables.

### Changed

- Refactored packing to docker: `--download-token` option is replaced with
  `--sdk-local` and `--sdk-path` options.
- Refactored RPM and DEB scripts (pre- and post- install, ExecStartPre in systemd
units) to be platform independent.

## [1.4.1] - 2020-03-06

### Changed

- Improved arguments parsing:
  * boolean flags `--flag` shouldn't be passed after all other options;
  * Both `--long_opt` and `--long-opt` patterns can be used, it will be parsed
    as `long_opt` option

### Fixed

- Docker error on placing dockerfile not within the build context
- Creating files owned by root on local machine when building application in docker

## [1.4.0] - 2020-02-05

### Added

- Allow to pass directory to build application in using `CARTRIDGE_BUILDDIR`
  environment variable
- `cartridge build` command to build application locally

### Changed

- By default, temporary directory for application building is created in
  `~/.cartridge/tmp`
- Commands usage messages are prettified
- `path` argument for `cartridge pack` command isn't required.
  By default, current directory is used.

### Fixed

- Delayed messages on application packing

## [1.3.2] - 2020-01-23

### Changed

- Common packing flow parameters are stored in the global `pack_state`
- Update cartridge to 2.0.1 in template
- Update luatest to 0.5.0
- Add luacov to app template

### Fixed

- Error on runnning `git clean` for submodules on `cartridge pack`

## [1.3.1] - 2020-01-20

### Added

- Allow to pass `--version` in format `major.minor.patch[-count][-hash]`

### Changed

- RPM header: added `PAYLOADDIGEST` and `PAYLOADDIGESTALGO` flags,
  removed `RPMVERSION`.

## [1.3.0] - 2020-01-13

### Added

- Packing to Docker image
- Check filemodes before packing
- `--from` option for `docker pack` command to specify base image Dockerfile path
- `cartridge.pre-build` and `cartridge.post-build` hooks
  to be ran before and after `rocks make`
- Deprecated build flow (`.cartridge.ignore` + `.cartridge.pre`) is supported
  for all distribution types except `docker`
- Recursively cleaning all submodules on application packing

### Changed

- `docker pack` log messages are coloured
- Pre-build, build and post-build actions are grouped in one RUN directive
  on packing to Docker image
- Update luatest to 0.4.0
- Freeze cartridge 2.0.0 in template

### Fixed

- Error on using environment variables in base Dockerfile
- Error on using COPY instruction in base Dockerfile
- Added missing environment variable `TARANTOOL_APP_NAME`
- Fix parsing options priority to match `cartridge.argparse`. Current parsing priority:
  firstly commandline options, then environment variables, then options from
  `.config.yml`, in the end default options. Options from `.config.yml` are
  overriden by corresponding to them environment variables.
- Error on rocks manifest processing

## [1.2.1] - 2019-11-25

- Fix building RPM package on CentOS 8
- Fix starting foreground apps with current Tarantool

## [1.2.0] - 2019-11-15

### Added

- luacheck in examples and templates
- `--version` option to display version
- Default cartridge-cli configuration in getting-started template
- Use current tarantool executable to start instance

### Changed

- Warnings in log are shown with yellow color
- `cartridge start` starts instances in foreground, `--foreground` is replaced with `--daemonize`

### Removed

- `plain` template

## [1.1.0] - 2019-10-24

### Added
- Start and stop all instances
- Start/stop instances defined in multiple files
- Colorized logs and prefix with instance name for multiple foreground instances
- Packing DEB

### Changed
- Disabled jit in tests until tarantool/tarantool#4476 is fixed
- Getting started app READMEs improved

### Fixed
- Luacheck warnings
- Missing setsearchroot in 1.10
- /var/run dir removal after reboot

## [1.0.0] - 2019-09-02

### Added

- Basic functionality
- Integration tests
- End-to-end tests
- Gitlab CI tests for opensource and enterprise Tarantool
- Packing RPM with Tarantool dependency for opensource
- Loading templates from .rocks
- Configuring systemd units using `cartrigde.argparse` way
- Getting started app
- Start and stop commands
- Cache downloaded sdk between ci jobs
