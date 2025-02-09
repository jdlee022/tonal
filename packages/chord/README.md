# @tonaljs/chord ![tonal](https://img.shields.io/badge/@tonaljs-chord-yellow.svg?style=flat-square) [![npm version](https://img.shields.io/npm/v/@tonaljs/chord.svg?style=flat-square)](https://www.npmjs.com/package/@tonaljs/chord)

## Usage

ES6:

```js
import { Chord } from "tonal";
```

Nodejs:

```js
const { Chord } = require("tonal");
```

## API

#### `Chord.getChord(type: string, tonic?: string, root?: string) => Chord`

Get the chord properties from a chord type and an (optional) tonic and (optional) root. Notice that the tonic must be present if the root is present. Also the root must be part of the chord notes.

It returns the same object that `ChordType.get` but with additional properties:

- symbol: the chord symbol (a combiation of the tonic, chord type shortname and root, if present). For example: `Cmaj7`, `Db7b5/F`. The symbol always uses pitch classes (note names without octaves) for both the tonic and root.
- tonic: the tonic of the chord (or an empty string if not present)
- root: the root of the chord (or an empty string if not present)
- rootDegree: the degree of the root. 0 if root not present. A number greater than 0 if present, where 1 indicates the tonic, 2 the second note (normally the 3th), 2 the third note (normally the 5th), etc.
- notes: an array of notes, or empty array if tonic is not present. If the root is pres

Example:

```js
Chord.getChord("maj7", "G4", "B4"); // =>
// {
//   empty: false,
//   name: "G major seventh over B",
//   symbol: "Gmaj7/B",
//   tonic: "G4",
//   root: "B4",
//   rootDegree: 2,
//   setNum: 2193,
//   type: "major seventh",
//   aliases: ["maj7", "Δ", "ma7", "M7", "Maj7"],
//   chroma: "100010010001",
//   intervals: ["3M", "5P", "7M", "8P"],
//   normalized: "100010010001",
//   notes: ["B4", "D5", "F#5", "G5"],
//   quality: "Major",
// }
```

#### `Chord.get(name: string) =>`

An alias of `Chord.getChord` but accepts a chord symbol as parameter.

```js
Chord.get("Cmaj7");
// same as
Chord.getChord("maj7", "C");
```

Important: currently chord with roots are NOT allowed (will be implemented in next version):

```js
Chord.get("Cmaj7/E"); // => { empty: true }
```

#### `Chord.detect(notes: string[]) => string[]`

Given a list of notes, get the possible chord names:

```js
Chord.detect(["D", "F#", "A", "C"]); // => ["D7"]
Chord.detect(["F#", "A", "C", "D"]); // => ["D7/F#"]
```

Alias of [chord-detect](/packages/chord-detect)

#### `Chord.transpose(chordName: string, intervalName: string) => string`

Transpose a chord symbol by an interval:

```js
Chord.transpose("Eb7b9", "5P"); // => "Bb7b9"
```

#### `Chord.chordScales(chordName: string) => string[]`

Get all scales where the given chord fits:

```js
Chord.chordScales("C7b9");
// => ["phrygian dominant", "flamenco", "spanish heptatonic", "half-whole diminished", "chromatic"]
```

#### `Chord.extended(chord: string) => string[]`

Get all chords names that are a superset of the given one (has the same notes and at least one more)

```js
Chord.extended("Cmaj7");
// => [ 'Cmaj#4', 'Cmaj7#9#11', 'Cmaj9', 'CM7add13', 'Cmaj13', 'Cmaj9#11', 'CM13#11', 'CM7b9' ]
```

#### `Chord.reduced(chord: string) => string[]`

Find all chords names that are a subset of the given one (less notes but all from the given chord)

```js
Chord.reduced("Cmaj7"); // => ["C5", "CM"]
```
