# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.3.0](https://github.com/beam-telemetry/telemetry_poller/tree/v0.3.0)

This release marks the upgrade to Telemetry 0.4.0. This means that Poller measurements can emit a map
of values now instead of a single one, making it less "noisy" when it comes to number of emitted events.

All specific memory measurements have been replaced with a single `:memory` measurement sending all
the values that were emitted by old measurements at once.

`:run_queue_lengths` VM measurement has been removed for now, as we believe that as detailed data
as it provided is not necessary to effectively debug the system. `:total_run_queue_lengths` VM
measurement has been changed so that it reports a `:total` length of run queues, length of `:cpu`
run queues (including dirty CPU run queue), and length of (dirty) `:io` run queue.

### Added

- `:memory` VM measurement reporting all the data returned by `:erlang.memory/0` call.

### Changed

- `:total_run_queue_lengths` VM measurement is reporting a `:total`, `:cpu` and `:io` run queue lengths
  now. See documentation for more details.

### Removed

- `:total_memory`, `:atom_memory`, `:atom_used_memory`, `:processes_memory`, `:processes_used_memory`,
  `:binary_memory`, `:ets_memory`, `:code_memory` and `:system_memory` VM measurements have been removed.
  Please use the `:memory` measurement now instead.

## [0.2.0](https://github.com/beam-telemetry/telemetry_poller/tree/v0.2.0)

### Added

- Added `:total_run_queue_lengths` and `:run_queue_lengths` memory measurements;

### Changed

- `:total_run_queue_lengths` is now included in the set of default VM measurements;
- A default Poller process, with a default set of VM measurements, is started when `:telemetry_poller`
  application starts (configurable via `:telemetry.poller, :default` application environment).
- VM measurements are now provided to Poller's `:vm_measurements` option. Passing atom `:default`
  to this option makes the Poller use a default set of VM measurements;
- Telemetry has been upgraded to version 0.3, meaning that VM measurements now use this version to
  emit the events.

### Removed

- `Telemetry.Poller.vm_measurements/0` function has been removed in favor of `:vm_measurements`
  option.

### Fixed

- Fixed the type definition of `Telemetry.Poller.measurement/0` type - no Dialyzer warnings are
  emitted when running it on the project.

## [0.1.0](https://github.com/beam-telemetry/telemetry_poller/tree/v0.1.0)

### Added

- The Poller process periodically invoking registered functions which emit Telemetry events.
  It supports VM memory measurements and custom measurements provided as MFAs.
