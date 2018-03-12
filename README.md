# Beatmap Visualization Server Protocol

## What is the Beatmap Visualization Server Protocol?

[Rhythm games](https://en.wikipedia.org/wiki/Rhythm_game) have a variety of visual and operational representations. Beatmaps, as the core data structure of rhythm games, are tailored for the game(s) they are designed for. Traditionally, to make beatmaps for a game, there have to be a specific software designed for the best design experience and efficiency, only for that game. Fan-made softwares, including beatmap editors and simulators, may use their project or record formats, in addition to official and most recognized formats.

Sadly, these beatmap editors and simulators are still isolated, so it is not possible to write a beatmap in beatmap editor A, and directly display it in simulator B (like debugging a program). You have to save your work, export it, maybe then convert it, and finally use the simulator to load and run the beatmap. If you want an in-editor preview, you have to copy the code from a simulator, which breaks the [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) rule.

Inspired by Microsoft's [Language Server Protocol](https://github.com/Microsoft/language-server-protocol) (LSP), we introduce the Beatmap Visualization Server Protocol (BVSP). BVSP is a standard way that beatmap editors and simulators communicate. It narrows down the complexity of cross-editor-and-simulator support. A Beatmap Visualization Server, embedded in an editor or simulator (depending on its role), can be reused. In addition, if the server follows the standard format for its targeted game, then in theory it supports all editors/simulators that are compatible with that game.

## Specification

See [here](protocol.md).

The specification is in a very early stage. Comments, suggestions, discussions, and other forms of contributions are all welcome!

## About Beatmap Formats

TBD

Due to the complexity of rhythm games, it is also not possible to have a common exchange format. At the time of writing, the most common format is BMS, a format for [Beatmania](https://en.wikipedia.org/wiki/Beatmania). However, this format abstracts the play area as several tracks, and accepts tap and hold (turn) inputs, which is not always the case in other rhythm games. The reason why BMS is popular is that in non-core rhythm games (e.g. for general population), you only have to tap the input device (e.g. screen), like [LLSIF](https://lovelive-sif.bushimo.jp/). So those games only need to support a subset of BMS. For more complicated games, like [osu!](https://osu.ppy.sh/), [CGSS](http://cinderella.idolmaster.jp/sl-stage/), [Arcaea](https://arcaea.lowiro.com/), or [Cytus](https://www.rayark.com/g/cytus/), there are featured notes that cannot be represented in standard BMS.

## License

Creative Commons Attribution / MIT
