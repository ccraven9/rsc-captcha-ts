# rsc-captcha-ts

> **Fork notice:** This is a fork of
> [`2003scape/rsc-captcha-ts`](https://github.com/2003scape/rsc-captcha-ts),
> maintained by [@ccraven9](https://github.com/ccraven9) as part of a
> modernization effort for the 2003scape ecosystem.

Generates RuneScape Classic sleep screen words. Players enter the generated
word during the [fatigue](https://classic.runescape.wiki/w/Fatigue) system to
continue gaining experience.

![Example captchas](./captchas.gif?raw=true)

## Install

From GitHub:

```sh
npm install github:ccraven9/rsc-captcha-ts
```

The package is still named `@2003scape/rsc-captcha-ts` (see `package.json`), so
after installing from GitHub it is required as:

```js
const Captcha = require('@2003scape/rsc-captcha-ts');
```

> **Note:** `canvas` is a native dependency. On modern Node.js versions it may
> fail to install without build tools; upgrading it is part of the fork's
> modernization goals.

## Example

```javascript
const Captcha = require('@2003scape/rsc-captcha-ts');
const captcha = new Captcha();
const fs = require('fs');

(async () => {
    await captcha.loadFonts();

    for (let i = 0; i < 10; i += 1) {
        const { word, image } = captcha.generate();

        if (process.browser) {
            document.body.appendChild(new Text(word));
            document.body.appendChild(image);
            document.body.appendChild(document.createElement('br'));
        } else {
            fs.writeFileSync(`${word}.png`, image.toBuffer());
        }
    }
})();
```

## API

### Captcha.toByteArray(canvas)

Converts any canvas into a game client-compatible RLE byte array.

### captcha = new Captcha(words?)

Creates a new captcha generator instance. `words` is an array of words to
randomly select from (defaults to `en-uk`, with word lengths between 4 and 9).

### async captcha.loadFonts()

Loads the main font image (`fonts.png`). Run before generating captchas.

### captcha.generate()

Generates a word and captcha associated with it. Returns:

```javascript
{
    image: Canvas,
    word: String
}
```

### captcha.getWord()

Picks a random word from the dictionary.

### captcha.generateImage(word)

Creates a Canvas with the specified word.

## License

Copyright 2020 2003Scape Team


This program is free software: you can redistribute it and/or modify it under
the terms of the GNU Affero General Public License as published by the
Free Software Foundation, either version 3 of the License, or (at your option)
any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License along
with this program. If not, see http://www.gnu.org/licenses/.
