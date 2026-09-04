# Bootstrap Contract

Load this reference when selecting an official base, configuring generated code, or deciding whether the empty project is complete.

## Input contract

Confirm only facts that affect generated output or verification:

- exact MCU ordering code and board revision;
- bare metal or exact RTOS/port;
- vendor SDK and configuration/generation tool version;
- compiler, build system, IDE/CLI, and debugger;
- external memories, clocks, and board facts required by the empty template;
- an observable boot signal acceptable to the user.

The user selects the stack. When a value is missing, ask the smallest blocking question instead of presenting a long questionnaire.

## Official-base decision

1. Exact official board/project example.
2. Exact MCU family example requiring documented board adaptation.
3. RTOS/vendor generator output for the exact device.
4. Minimal official SDK scaffold.
5. From scratch only with an explicit explanation that no suitable official base exists.

Never combine unrelated examples to appear complete. Record the source path or URL, version, and any adaptation.

## Generated-code ownership

Classify files before editing:

- **Generator-owned:** regenerate through the configuration tool.
- **Protected user region:** edit only inside documented preservation markers.
- **Application-owned:** keep custom modules here.
- **Vendor source:** prefer configuration, wrappers, or patches that remain traceable.

For an unavoidable generator-owned edit, present:

1. the exact file and generated owner;
2. why configuration/user regions cannot express the change;
3. the smallest patch;
4. regeneration and upgrade risk;
5. an alternative;
6. a request for explicit approval.

## Dependency-layout recommendation

Do not impose a universal layout. Compare only realistic options for the selected ecosystem:

| Option | Prefer when | Main risk |
|---|---|---|
| Official package manager | The ecosystem pins and restores reliably | Registry/tool availability |
| Git submodule | Upstream history and explicit revision matter | Submodule workflow friction |
| Vendored source | Offline/reproducible build is essential and license permits | Repository size and upgrade burden |
| External pinned SDK | Vendor tools expect a workspace SDK | Machine setup and path reproducibility |

Wait for the user to choose after the recommendation.

## Verification contract

Run a clean build from a documented entry point. Confirm:

- tool and SDK versions match the recorded contract;
- regeneration does not erase application-owned changes;
- no accidental absolute developer paths are required;
- the intended image is produced;
- warnings/errors and memory reports are stated honestly.

With hardware, separate three observations:

1. Programmer connected to the intended target.
2. Intended image downloaded and verified.
3. Firmware visibly booted.

Do not collapse them into one “works” claim.
