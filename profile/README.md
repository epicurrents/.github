Epicurrents
============

Epicurrents is an open-source JavaScript/Typescript library for reading, processing and visualizing neurophysiological signal data in a web browser. It is modular in design and already supports multiple study modalities and source file formats.

The library is aimed at medical educators and scientists looking for an accessible, cost-effective and extensible tool for making real-life neurophysiological data available to others. It is not a medical device and should not be used for clinical diagnostics.

Core capabilities
-----------------
- Multi-modality study support (EEG, EMG, NCS, tabular data, etc.).
- Pluggable file readers for common biomedical and document formats (EDF, DICOM, PDF, Markdown/HTML, WAV).
- Worker-based processing for heavy tasks (memory manager, montage computation, Pyodide-based compute).
- A Vue/Vite-based user interface that can be used as a standalone app or imported as a library into a host application.
- Extensible services (Pyodide, ONNX) for analytics and model-backed processing.

Project structure and components
--------------------------------
The codebase is organized so individual capabilities live in separate packages that are coordinated by the
`@epicurrents/core` runtime. Key component categories:

- Core (`core`): the application runtime and API. It manages module registration, worker overrides, services, and the lifecycle of studies and datasets. Modules and readers register through the core API.

- Interface (`interface`): the Vue/Vite UI package. It implements the user-facing controls, views and the glue that connects the core to the web frontend. The interface can be built as a standalone web app (app bundle) or as a consumable library that a host application can register into the `Epicurrents` core.

- Study modules (e.g. `eeg-module`, `emg-module`, `ncs-module`, `tab-module`): domain-specific viewers and tools for a modality. Modules provide loaders, resource classes and processing hooks.

- File readers (e.g. `edf-reader`, `dicom-reader`, `htm-reader`, `pdf-reader`, `wav-reader`): format-specific importers and optional worker implementations used to read and parse files.

- Services (e.g. `pyodide-service`, `onnx-service`): background services that provide computation or model execution capabilities. Services move heavy workloads from the main application processor thread to Web Workers or remote servers. Services are registered into the core and can be transparently used by modules.

How components interact
-----------------------
- The `core` exposes APIs for registering modules, services and study importers.
- The `interface` registers itself with the `core` (via `coreApp.registerInterface`) and uses the `core` API to present study lists, import dialogs and visualization components.
- Modules register study loaders and importers (`coreApp.registerStudyImporter`) so the UI can open files from disk, URL or folder and present them using the module's viewers.
- Worker code (memory manager, montage, file parsing) is registered/overridden through `coreApp.setWorkerOverride`. The default interface entry script demonstrates common patterns for setting these overrides.

Use in medical education and research
-------------------------------------
Epicurrents is designed as an extensible platform for exploring biosignals and related documents. Typical applications include:

- Teaching: instructors can build interactive demonstrations that load sample studies and annotate signals.
- Research: studies and recordings in different formats can be viewed and annotated in a single UI. Researchers can run analyses using Python or machine learning models on recordings and visualize the results alongside signals and metadata.
- Prototyping: new file readers, analysis services or visualization modules can be developed independently and integrated via the core API.

Interface example and documentation
-----------------------------------
- The `interface` repository demonstrates how to wire up modules, workers, services and the UI. See `src/setups/default.ts` for a concrete example of registering modules and worker overrides programmatically.
- Further documentation and usage examples are available in the online docs: https://docs.epicurrents.io

Docs and examples
-----------------
- API and user documentation: https://docs.epicurrents.io
- Inspect the `interface` example and the `src/setups/standalone.ts` entry script for concrete module and worker registration patterns.

License
-------
Epicurrents packages are licensed under Apache-2.0 (see individual package `LICENSE` files for details).
