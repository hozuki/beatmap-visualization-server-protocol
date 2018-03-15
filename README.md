# Beatmap Visualization Server Protocol

## What is the Beatmap Visualization Server Protocol?

[Rhythm games](https://en.wikipedia.org/wiki/Rhythm_game) have a variety of visual and operational representations. Beatmaps, as the core data structure of rhythm games, are tailored for the game(s) they are designed for. Traditionally, to make beatmaps for a game, there have to be a specific software designed for the best design experience and efficiency, only for that game. Fan-made softwares, including beatmap editors and simulators, may use their project or record formats, in addition to official and most recognized formats.

Sadly, these beatmap editors and simulators are still isolated, so it is not possible to write a beatmap in beatmap editor A, and directly display it in simulator B (like debugging a program). You have to save your work, export it, maybe then convert it, and finally use the simulator to load and run the beatmap. If you want an in-editor preview, you have to copy the code from a simulator, which breaks the [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) rule.

Inspired by Microsoft's [Language Server Protocol](https://github.com/Microsoft/language-server-protocol) (LSP), we introduce the Beatmap Visualization Server Protocol (BVSP). BVSP is a standard way that beatmap editors and simulators communicate. It narrows down the complexity of cross-editor-and-simulator support. A Beatmap Visualization Server, embedded in an editor or simulator (depending on its role), can be reused. In addition, if the server follows the standard format for its targeted game, then in theory it supports all editors/simulators that are compatible with that game.

## Specification

See [here](protocol-draft.md).

The specification is a draft in a very early stage. Comments, criticisms, suggestions, discussions, and other forms of contributions are all welcome!

## Proposals

[List of Proposals](proposals)

## License

Creative Commons Attribution / MIT
