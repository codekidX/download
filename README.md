# download

<!-- [![Travis]()]() -->

A simple deno `fetch api` based module to `download` file from a URL.

## Import

```ts
import { download } from "./mod.ts";
```

## Usage

##### SAMPLE 1 :
``` ts
import { download } from "./mod.ts";
// import { DownlodedFile } from "./lib.d.ts"

const url = 'https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf';

try {
    const fileObj = await download(url);
    // const fileObj: DownloadedFile = await download(url);
} catch (err) {
    console.log(err)
}
```
##### Alternatively :
``` ts
import { download } from "./mod.ts";

const url = 'https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf';

download(url)
  .then(fileObj => {
    console.log(fileObj)
  })
  .catch(err => {
    console.log(err)
  });
```
By default, the module creates a temporary directory every time you call the `download` function and downloads the file into it.

You can specify the download destination, filename and also the file permission via the second parameter.

**Note :**` download directory should be present, else error will be thrown`


``` ts
// def of 2nd parameter. ./lib.d.ts
Destination {
  dir?: string,
  file?: string,
  mode?: number
}
```
##### SAMPLE 2 :
``` ts
import { download, Destination } from "./mod.ts";

const url = 'https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf';

try {
    // NOTE : You need to ensure that the directory you pass exists.
    const destination: Destination = {
        file: 'example.pdf',
        dir: './test'
    }
    /* sample with mode
     const destination: Destination = {
        file: 'example.pdf',
        dir: './test',
        mode: 0o777
    }
    */
    const fileObj = await download(url, destination);
    // const fileObj: DownloadedFile = await download(url,destination);
} catch (err) {
    console.log(err)
}
```
##### Passing http methods and headers:
Behind the scene this module uses deno's fetch api. The third parameter to download() function is [RequestInit](https://github.com/denoland/deno/blob/master/cli/js/lib.deno.shared_globals.d.ts#L769). You can pass body, headers, cache, method.. the same way you pass to the fetch api.

##### SAMPLE 3 :
``` ts
import { download, Destination } from "./mod.ts";

const url = 'https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf';

try {
    const destination: Destination = {}
    const reqInit: RequestInit = {
        method: 'GET'
    }
    /* sample with mode
     const destination: Destination = {
        file: 'example.pdf',
        dir: './test',
        mode: 0o777
    }
    */
    const fileObj = await download(url, destination, reqInit);
    // const fileObj: DownloadedFile = await download(url,destination);
} catch (err) {
    console.log(err)
}
```
### Return:
`download` function returns `file`(filename), `dir`, `fullPath`, and `size`(in bytes)
```ts
// definiton of return object. check:./lib.d.ts
DownlodedFile {
  file: string,
  dir:string,
  fullPath: string,
  size: number
}
```

## Deno Permission
You need `--allow-net` and `--allow-write` permission to use `download` function.

## License

[MIT](./LICENSE)