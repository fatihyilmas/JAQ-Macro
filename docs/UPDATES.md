# Downloads and updates

## Windows download

Version 2.0.27 is available as a portable ZIP for Windows x64. Extract all files into a writable folder, keep the `lang` folder next to `JAQ-MACRO.exe`, and open the executable. The desktop build requests Windows administrator approval. `SHA256SUMS.txt` lists the ZIP checksum.

[Latest release](https://github.com/fatihyilmas/JAQ-Macro/releases/latest)

## In-application updates

1. JAQ Macro checks GitHub Releases for a newer application package. The version control opens installed version information and shows when an update is available.
2. If you select Download, JAQ Macro verifies the publisher signature, ZIP size and checksum, and the declared file list.
3. Installation requires separate confirmation and an idle, saved workspace. JAQ Macro stages the verified files, applies them through a helper and restarts. The updater keeps a recovery backup for failures.

The ECDSA P-256 signature in `jaq-update.json` authenticates the update descriptor and its file hashes using the publisher public key pinned in the app. Windows Authenticode signing is separate and this portable EXE has no Authenticode signature.

The older `2.0.12-preview.1` release remains a documentation-only channel test. Its ZIP contains Markdown files and extracts into a separate `Update-Test` folder when used through the updater.
