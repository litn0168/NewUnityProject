# Pull Request

## Summary
<!-- What changes, and why. One or two sentences. -->

## Changed files
<!-- Explicit list. Do not write "several files". -->
- `path/to/File.cs` — what changed

## Game systems / scenes / prefabs affected
<!-- Name the gameplay systems touched. -->
-
- Scene or prefab touched? **yes/no** — if yes, list every one and note that
  Unity YAML conflicts are resolved in the Editor, never by hand-merging:
  -

## Tests actually run
<!-- Command + observed output. Only what you really executed. -->
```
$ <command>
<observed output>
```

## Tests NOT run
<!-- Be explicit; do not leave this blank. -->
- [ ] Unity Editor compile — reason:
- [ ] Play Mode smoke test — reason:
- [ ] EditMode / PlayMode test suite — reason:

## Risks / rollback
- **Risk:**
- **Rollback:** revert commits

## Checklist
- [ ] No `Library/`, `Temp/`, `Logs/`, `UserSettings/`, `obj/`, `Build/`,
      `.vs/`, `*.csproj`, `*.sln(x)` staged
- [ ] `.vsconfig` is allowed/intentional (shared IDE component list);
      `sysinfo.txt` and `crashlytics-build.properties` must not be staged
- [ ] No `.meta` files deleted or rewritten (GUIDs preserved)
- [ ] No scene/prefab GUID or fileID churn beyond the intended change
- [ ] `ProjectSettings/` unchanged, unless stated above
- [ ] `Packages/manifest.json` unchanged, unless stated above
- [ ] Branch is not `main`; commits staged by explicit path (no `git add .`)
