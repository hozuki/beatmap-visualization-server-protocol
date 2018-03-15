# Proposal: Format Negotation

## Introduction

LSP's text document model is plain and simple, just a 2D matrix of characters. Almost all source code documents are text documents. Therefore, in LSP, server and client do not need to negotiate for a common document format/model.

However, in beatmaps for rhythm games, the situation is not the same. For a certain game, there may be several different formats used in editors and simulators. Different formats seldom share the same content model abstraction.

Due to the complexity of rhythm games, it is also not possible to have a common exchange format. At the time of writing, the most common format is BMS, a format for [Beatmania](https://en.wikipedia.org/wiki/Beatmania). However, this format abstracts the play area as several tracks, and accepts tap and hold (turn) inputs, which is not always the case in other rhythm games. The reason why BMS is popular is that in non-core rhythm games (e.g. for general population), you only have to tap the input device (e.g. screen), like [LLSIF](https://lovelive-sif.bushimo.jp/). So those games only need to support a subset of BMS. For more complicated games, like [osu!](https://osu.ppy.sh/), [CGSS](http://cinderella.idolmaster.jp/sl-stage/), [Arcaea](https://arcaea.lowiro.com/), or [Cytus](https://www.rayark.com/g/cytus/), there are featured notes that cannot be represented in standard BMS.

To solve the problem, we propose the format negotiation mechanism. It requires the editor and the simulator have support for the same beatmap document format (may be the most popular ones). When the common format is selected, the editor will always export this format, and the simulator will always expect this format to read.

## Procedure

The negotiation happens in simulator initialization (`general/simInitialize` request). It goes through three stages:

1. The editor (**E**) lists all *output* formats it supports. In the `general/simInitialize` request, **E** sends the list of supported formats to the simulator (**S**).
2. When **S** initializes, it lists all *input* formats it supports, and cross compare it with the list **E** sent. **S** will try to select the first format they both contain; if there is no such format, the request fails. The selected format is acknowledged in the response of simulator initialization. For initialization failure, see the description at `general/simInitialize`.
3. **E** validates the response. If the request succeeds, it will use the selected format, until launching next simulator. Request failure handling remains default.

## Data Structures

### `FormatDescriptor`

```ts
interface SupportedFormatDescriptor {
    // Game ID
    game: string;
    // Format ID
    id: string;
    // Format version list
    versions: string[];
    // Extra info
    extra?: any;
}

interface SelectedFormatDescriptor {
    // Game ID
    game: string;
    // Format ID
    id: string;
    // Format version
    version: string;
    // Extra info
    extra?: any;
}
```

A `FormatDescriptor` is used to describe a beatmap file format. `game` denotes the ID of targeted game, `id` denotes the format ID, and `version` is the format version. Extra information can be stored in `extra`, if the corresponding format needs those information. `version`'s data type is set to string to support non-integer version numbers (e.g. `1.0a`).

Game ID should be unique for each rhythm game. An example list:

| Game | ID |
|---|---|
| [THE iDOLM@STER Cinderella Girls: Starlight Stage](http://cinderella.idolmaster.jp/sl-stage/) | `deresute` |
| [osu!](https://osu.ppy.sh/) | `osu` |
| [BanG Dream!](https://bang-dream.com/) | `bang_dream` |
| ... | ... |

Format ID should be unique in the context of a game. An example list (of Starlight Stage):

| Format | ID |
|---|---|
| CSV Beatmap (used by official app) | `csv` |
| Deleste TXT | `txt` |
| ... | ... |

Given a format ID, there can be several versions in the version list which is not empty. However, in the selected format, the exact version is also determined. The selected version is, but not necessarily needs to be, the highest version that both the editor and the simulator support.

The list of game IDs and format IDs should be maintained separately for maximum backward compatibility, and subjecting to appending new records.

### Modifications to Request Parameter

```ts
interface GeneralSimInitializeRequestParameter {
    // ...
    supportedFormats: SupportedFormatDescriptor[];
    // ...
}
```

### Modifications to Response Object

```ts
interface GeneralSimInitializeResponseObject {
    // ...
    selectedFormat: SelectedFormatDescriptor;
    // ...
}
```
