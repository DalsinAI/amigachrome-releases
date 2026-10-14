# AmigaChrome releases

Signed AmigaChrome patches for machines outside the Team: the `.deb` install,
Raspberry Pi cards and anyone else running AmigaChrome. There is no source
code here; the code lives in AmigaChrome's own repository.

The design is `docs/design/Design-Signed-Patch-Releases.md` in AmigaChrome's
repository. **Nothing is published yet.** The channels stay empty until
AmigaChrome's patch runner checks signatures.

## Channels

| Channel | For |
| --- | --- |
| `stable` | Everyone. Cradle's default once updates are switched on. |
| `beta` | Builds being tried before they go to `stable`. |
| `dev` | The Team's own test builds. |

Each channel has one file, `channels/<channel>.json`, listing the patches that
are current for it:

```json
{
  "schema": 1,
  "channel": "stable",
  "updatedAt": "2026-10-20T12:00:00Z",
  "patches": [
    {
      "name": "acpatch-<title>-<YYYYMMDDTHHMMSSZ>.zip",
      "sha256": "<64 hex>",
      "size": 123456,
      "url": "https://github.com/DalsinAI/amigachrome-releases/releases/download/<tag>/<name>",
      "sig": "https://github.com/DalsinAI/amigachrome-releases/releases/download/<tag>/<name>.sig",
      "minVersion": "0.30.0",
      "maxVersion": null,
      "targets": null
    }
  ]
}
```

Once signing is in, every channel file has a signature beside it,
`channels/<channel>.json.sig`. Each patch is a GitHub Release asset with its
own `.sig`, under a tag `<channel>-<build id>`.

## Checking a patch yourself

Patches are signed with OpenSSH signatures (`ssh-keygen -Y`) in the
namespace `amigachrome-patch`. The Team's public keys are in AmigaChrome's
`config/patch-signers`. To check one by hand:

```sh
ssh-keygen -Y verify -f patch-signers -I release@amigachrome \
  -n amigachrome-patch -s acpatch-NAME.zip.sig < acpatch-NAME.zip
```

AmigaChrome checks every patch this way before it runs anything. A patch that
doesn't verify is refused.

## Licence

The patches carry AmigaChrome's own licence. This repository's text: Copyright
(c) 2026 Dalsin Limited. MIT.
