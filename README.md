# WASM in IPLD

This is a simple library for:

* Encoding the results of WASM functions alongside a reference to that function.
* Encoding and decoding WASM functions that are larger than a single block [using FBL](https://github.com/ipld/specs/blob/master/data-structures/flexible-byte-layout.md).
* Caching WASM functions that have already been loaded.

```js
import create from '@ipld/wasm'

// example block store
const get = async cid => kv.get(cid).then(encodeBlock)
const put = async ({ cid, bytes }) => kv.put(cid.toString(), bytes)

// example WASM encoders and decoders
const encode = async (wasm, ...args) => {
  // encode args, use wasm function, etc
  // must return single CID for encoding
  return cid
}
const decode = async (wasm, cid) => {
  const block = await get(cid)
  // do wasm things and return any JS object you'd like
}

const iw = create({ get, put, encode, decode })

// get a content id for a WASM binary
const cid = await iw.wasm(bufferOrStreamOfWASMBinary)

// run the given WASM binary
const block = await iw.encode(cid, 'test')

/* seperate file */

const iw = create({ get, put, encode, decode })

// You can now decode the result, iw will
// load the result data and the wasm function
// (potentially from a network) using the `get`
// function and passed into your decoder.
const decoded = await iw.decode(block.cid)
```
