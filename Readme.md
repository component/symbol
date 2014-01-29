# symbol

  This component implements ECMAScript 6-like `Symbol`s in ECMAScript 5. 
  A `Symbol` can be used as a unique key for setting non-enumerable (i.e. "invisible") properties on objects.
  This is specially useful for implementing truly private stores in JS objects. This component can be used
  both on its own or as a Polyfill for ES6 `Symbol`s.

    var symbol = require('symbol');

    var obj = { name: 'John Doe' };

    (function() {
      var privateStore = symbol();

      obj[privateStore] = {
        secretAgent: true,
        realName: 'James Bond'
      }
    })();

    // this will not expose privateStore
    for (var key in obj) {
      console.log(key);
    }

## Installation

  Install with [component(1)](http://component.io):

    $ component install component/symbol

## API

### symbol()

Creates a new symbol.

## Usage as a polyfill for native Symbols

    if (typeof window.Symbol == 'undefined') {
      window.Symbol = require('symbol');
    }

## Implementation Considerations

The implementation is based on an example on the WebReflection blog, by Andrea Giammarchi:

http://webreflection.blogspot.com/2013/03/simulating-es6-symbols-in-es5.html

(The original example implementation is MIT licensed, and so is this component.)

The "trick" for getting a non-enumerable property when using a `Symbol` as a key — without
having to manually resort to Object.defineProperty() every time — is defining for each symbol
a property on the global `Object` prototype, that's responsible for calling Object.defineProperty()
for us when set. This is not as bad as it sounds, as the property is both unique (using https://github.com/component/uid)
and non-enumerable.

### Memory Leak

**Important:** While collisions or changes in behavior will not be triggered by the presence of the
global property for the reasons stated above, its value will never be released, so this specific 
implementation will leak memory over time if used for a very large number of disposable Symbol objects.

## License

  The MIT License (MIT)

  Copyright (c) 2013 WebReflection / Andrea Giammarchi
  Copyright (c) 2014 Automattic, Inc.

  Permission is hereby granted, free of charge, to any person obtaining a copy
  of this software and associated documentation files (the "Software"), to deal
  in the Software without restriction, including without limitation the rights
  to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
  copies of the Software, and to permit persons to whom the Software is
  furnished to do so, subject to the following conditions:

  The above copyright notice and this permission notice shall be included in
  all copies or substantial portions of the Software.

  THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
  IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
  FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
  AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
  LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
  OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
  THE SOFTWARE.
