# file loader for webpack

## Usage

[Documentation: Using loaders](http://webpack.github.io/docs/using-loaders.html)

``` javascript
var url = require("file!./file.png");
// => emits file.png as file in the output directory and returns the public url
// => returns i. e. "/public-path/0dcbbaa701328a3c262cfd45869e351f.png"
```

By default the filename of the resulting file is the MD5 hash of the file's contents 
with the original extension of the required resource.

## Filename templates

You can configure a custom filename template for your file using the query
parameter `name`. For instance, to copy a file from your `context` directory
into the output directory retaining the full directory structure, you might
use `?name=[path][name].[ext]`.

### Filename template placeholders

* `[ext]` the extension of the resource
* `[name]` the basename of the resource
* `[path]` the path of the resource relative to the `context` query parameter or option.
* `[hash]` the hash of the content (or the hash source, if the `hashSource` query parameter is present; see below)
* `[<hashType>:hash:<digestType>:<length>]` optionally you can configure
  * other `hashType`s, i. e. `sha1`, `md5`, `sha256`, `sha512`
  * other `digestType`s, i. e. `hex`, `base26`, `base32`, `base36`, `base49`, `base52`, `base58`, `base62`, `base64`
  * and `length` the length in chars
* `[N]` the N-th match obtained from matching the current file name against the query param `regExp`

### Hash Source

* the `hashSource` parameter, if supplied, is used to generate the string that is hashed instead of the resource content
* the `hashSource` parameter works similar to the `name` parameter (above), in that it interpolates:
  * `[name]`
  * `[ext]`
  * `[path]`
  * `[N]`
  * `[hash]`, `[<hashType>:hash:<digestType>:<length>]`
* using `[hash]` here will always hash the actual file content, and *not* apply the `hashSource` parameter recursively

## Examples

``` javascript
require("file?name=js/[hash].script.[ext]!./javascript.js");
// => js/0dcbbaa701328a3c262cfd45869e351f.script.js

require("file?name=html-[hash:6].html!./page.html");
// => html-109fa8.html

require("file?name=[hash]!./flash.txt");
// => c31e9820c001c9c4a86bce33ce43b679

require("file?name=[sha512:hash:base64:7].[ext]!./image.png");
// => gdyb21L.png
// use sha512 hash instead of md5 and with only 7 chars of base64

require("file?name=img-[sha512:hash:base64:7].[ext]!./image.jpg");
// => img-VqzT5ZC.jpg
// use custom name, sha512 hash instead of md5 and with only 7 chars of base64

require("file?name=picture.png!./myself.png");
// => picture.png

require("file?name=[path][name].[ext]?[hash]!./dir/file.png")
// => dir/file.png?e43b20c069c4a01867c31e98cbce33c9
```

## License

MIT (http://www.opensource.org/licenses/mit-license.php)
