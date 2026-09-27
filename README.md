# OnHandBestFit

OnHandBestFit answers **WHO should handle this capability requirement?** It selects the best matching registered Provider and returns a portable result. It does not execute the selected Provider.

Supported Provider kinds are `skill`, `module`, and `cli`. Python 3.11 or newer is required.

## Install

Install the runtime wheel, then install or mount the `onhandbestfit` Agent Skill in your Host. This release intentionally does not publish an sdist or the engineering source tree.

```console
python -m pip install onhandbestfit-1.0.0-py3-none-any.whl
```

Before first use, initialize OnHandBestFit with the Host's currently registered, callable Provider inventory. The Host can refresh that inventory when it changes.

Minimal initialization input:

```json
{"mode":"bootstrap","host_id":"my-stable-host","providers":[{"provider_id":"skill.example","provider_name":"example","provider_kind":"skill","public_description":"Handles document OCR.","entry_ref":{"path":"/opt/host/skills/example/SKILL.md"},"registered":true,"callable":true,"capabilities":{"capability_need":["document.ocr"]}}]}
```

Run it through the mounted Skill:

```console
python scripts/invoke.py bootstrap.json
```

Normal task selection input:

```json
{"mode":"select","host_id":"my-stable-host","requirement":{"schema_version":"1.0","request_id":"request-1","route":"document","required":{"capability_need":"document.ocr"},"preferred":{}}}
```

Capability identifiers come from the Host's registered Provider inventory; they are not a universal fixed vocabulary.

The portable result broadly identifies a match, no match, or a need for narrowing, and always reports that no Provider was executed. Invalid, stale, insufficient, or contract-mismatched inputs fail closed.

## Scope and limitations

OnHandBestFit can run standalone and can be composed with other OnHand Skills, but it does not plan workflows, route execution, or run Providers. A Host must expose a stable identity and a truthful inventory of registered, callable Providers. WorkBuddy has completed real-Host validation. Full BestFit + Thoroughfare E2E and Drudge + BestFit + Thoroughfare E2E remain deferred.
