# docker-circleci-template

This is a simple template for building agile developed containers using CircleCI with auto versioning support.

<!-- MarkdownTOC depth=3 -->

- [Usage](#usage)
- [Building](#building)
  - [Versioning](#versioning)
- [Pushing](#pushing)
- [License](#license)

<!-- /MarkdownTOC -->


<a name="usage"></a>
## Usage

```
docker run \
  -d \
  -p 80:80 \
  parabuzzle/docker-circleci-template
```

<a name="building"></a>
## Building

```
rake build
```

<a name="versioning"></a>
### Versioning

This package uses an auto-incrementing build system. It uses the version in the `VERSION` file as the base version and searches for that version on Docker Hub using the Docker Hub api. It will take the current version on Docker Hub found for the base version provided in the `VERSION` file and automatically add `1` to it and build that next version. If not found it will append a `.0` to the `VERSION`.

examples:

```
base version = 0.1
current docker hub versions = [ 0.1.0, 0.1.1, 0.1.2 ]
version built will be = 0.1.3
###

base_version = 0.1.5
current docker hub versions = [ 0.1.0, 0.1.5, 0.1.5.0, 0.1.6, 0.2.1 ]
version built will be = 0.1.5.1
```

<a name="pushing"></a>
## Pushing

```
rake push
```

<a name="license"></a>
## License

The MIT License (MIT)

Copyright (c) 2015 Michael Heijmans

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
