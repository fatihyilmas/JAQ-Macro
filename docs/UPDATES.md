# Downloads and updates

## This release is a distribution test

The `2.0.12-preview.1` release is explicitly documentation-only. Its ZIP contains `README.md`, `LICENSE.md` and `UPDATE-TEST.md`. It has no EXE, DLL, installer, source code, license key or embedded account credentials. It is not a runnable application download.

## In-application flow

1. Check the public release metadata in the background, with cached checks and a non-blocking failure path when offline.
2. Show the installed version beside the logo. Mark newer packages and distinguish a documentation test from an actual application update.
3. Let the user download the ZIP. Verify the publisher signature, archive size/hash and declared file list.
4. Ask for extraction/installation consent separately. Declining leaves application files unchanged.
5. For this preview, extract documents into a separate `Update-Test` subdirectory. No application replacement or restart is required.
6. For a future application package, require an idle saved workspace, stage verified files, close the application and apply through a verified helper process. Keep a recovery backup and restore changed files if application fails.

## Publisher authenticity

The `jaq-update.json` release asset is an ECDSA P-256 signed descriptor. Its signature is verified against the public key pinned in the application. The archive digest and file hashes are authenticated by that signature. The private signing key is never included in the repository or assets.

Ordinary users should not edit the descriptor or rename package contents. Automatic installation rejects unsafe paths, protected user-data locations, unexpected entries and incorrect hashes. Workflows/settings are not intended to be overwritten by application packages.

Download and test only official assets from this repository. Source archives automatically shown by GitHub contain the public documentation repository, not the proprietary application source.
