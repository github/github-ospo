# Licenses for GitHub open source projects

You have a GitHub project that ought be open source per our [almost everything](XXX) guideline (not core business value, not specific to internal processes, and can be maintained long term) and the items on our [releasing open source task list](releasing.md) look feasible. One of checklist items is **It has a `LICENSE`**.

This document answers "which license?"

**TL;DR:**

- Code → [use MIT](#software)
- Not code (documentation, media) → [use CC-BY](#content)
- Snippets, data, boilerplate → [use CC0](#non-substantial-works)
- Something else, special circumstances → [open an issue in XXX](XXX)

## Software

Default: MIT

For most substantial GitHub open source software projects, there's a simple default answer: [MIT](https://choosealicense.com/licenses/mit/). MIT is the [most popular license](https://github.com/blog/1964-open-source-license-usage-on-github-com) for public open source projects on github.com. It has great cultural acceptance and is simple to understand, use, and comply with:

* Add a LICENSE.md file with the MIT license text and GitHub copyright notice: "Copyright (c) 2016 GitHub". You can do this via the web interface (a license picker will automatically appear when you add a new file called LICENSE.md) or by copying the license text from https://choosealicense.com/licenses/mit
* Keep that LICENSE.md file and copyright notice in any modified versions.

If you feel that MIT is really the wrong license for a substantial GitHub open source project, let's talk: XXX.

## Content

Default: CC-BY-4.0

Occasionally we publish substantial non-software content (e.g., documentation, media) that we want to give others permission to copy, modify, and distribute if they give us credit and don't use our trademarks. That's what CC-BY-4.0 allows. It's roughly equivalent to MIT in terms of being permissive and having good cultural acceptance, but is designed for non-software works (e.g., license notice can be provided with a link rather than including a copy of the license text).

To use CC-BY-4.0:

* In the case an entire repository should be released under CC-BY-4.0: Add a LICENSE.md file with the CC-BY-4.0 license text. It is unlikely you will want to do this, and it is not facilitated by the web interface license picker. The license text is available at https://creativecommons.org/licenses/by/4.0/legalcode.txt but check with @github/open-source to ensure this is what you really want.
* In the case particular files or parts of content (e.g., documentation or a media file) should be released under CC-BY-4.0, note this precisely in the repository's README.md ([example](XXX)).
* If the released material is rendered or published, e.g., as or in web pages, it can also be useful to include a CC-BY-4.0 notice there, e.g., "This documentation is released under CC-BY-4.0", with a link to https://creativecommons.org/licenses/by/4.0/ or the repository README.md#Licenses depending on the complexity of the situation. Please ask @github/open-source for help getting it right.
* To use material under CC-BY-4.0, license notice and attribution must be preserved. It can be useful to provide an example of how to do so ([example](XXX)).

If you feel that CC-BY-4.0 is really the wrong license for substantial GitHub open source non-software content, let's talk: XXX.

## Non-substantial works

Default: CC0-1.0

MIT and CC-BY-4.0 conditions are easy to comply with, but sometimes projects are better served by not having any conditions, not even a requirement for attribution.

[CC0-1.0](https://choosealicense.com/licenses/cc0-1.0/) waives all copyright restrictions but reserves trademark and patent rights, making it an easy unconditional license for GitHub material when:

* burden to user of maintaining copyright notices forever is large relative to incremental value of using licensed material, and
* there is no demonstrable business value from mandating maintenance of copyright notices

Examples:

1. Sample snippets; eg those under https://developer.github.com/guides/ discussed in XXX
2. Starter or other boilerplate material; eg XXX
3. Purely functional configuration with minimal expressivity that we don't think is/ought be subject to copyright anyway; add certainty that there are no restrictions; eg XXX
4. Data in which there is clearly no demonstrable business value from mandating maintenance of copyright notices
5. More substantial code/material alongside/in same repo as one of the above and still no demonstrable business value from restrictions; just use an unconditional license for more substantial bits to minimize number of licenses involved, eg  XXX

To use CC0-1.0:

* In the case an entire repository should be released under CC0-1.0: Add a LICENSE.md file with the CC0-1.0 license text. You can do this via the web interface (a license picker will automatically appear when you add a new file called LICENSE.md) or by copying the license text from https://choosealicense.com/licenses/cc0-1.0
* In the case particular files or parts of content (e.g., code snippets in documentation) should be released under CC0-1.0, note this precisely in the repository's README.md ([example](XXX)).
* If the released material is rendered or published, e.g., as or in web pages, it can also be useful to include a CC0-1.0 notice there, e.g., "Code samples in this documentation are released under CC0-1.0", with a link to https://creativecommons.org/publicdomain/zero/1.0/ or the repository README.md#Licenses depending on the complexity of the situation. Please ask @github/open-source for help getting it right.
* Note there are zero copyright license requirements when using material released under CC0-1.0, but it is usually best practice to maintain license notices and attribution anyway, as you would for MIT and CC-BY software and non-software above.

Have questions about whether what you're working on matches one of the above example classes and meets the two criteria above, or feel that CC0-1.0 isn't the right license for those cases? Let's talk: XXX.

Note that there are several other unconditional license options, including the Unlicense, Free Public License 1.0.0/0BSD, WTFPL, and ad hoc public domain dedications. CC0-1.0 is the preferred unconditional license for GitHub projects (see trademark and patents above), it isn't necessarily the right choice for substantial open source projects that want to use an unconditional license (in particular because of its explicit exclusion of any patent grant). That's why choosealicense.com's [unconditional/public domain category](https://choosealicense.com/licenses/) still defaults to Unlicense, and may in the future default to the Open Source Initiative [approved](https://opensource.org/licenses/FPL-1.0.0) Free Public License 1.0.0. Detailed discussion of unconditional licenses in XXX.

## Non-GitHub projects

While not the focus of this document, worth a mention:

* If you're contributing to an open source project that GitHub uses but is not the primary maintainer of, you will be contributing under the terms of whatever license the project uses.
* Make sure the project is under terms that GitHub can use. If the project license isn't a well-known permissive license (MIT, BSD, Apache 2.0), talk to your manager and XXX first. If contributing to the project requires accepting a CLA, ask on slack or create an issue in XXX repo. We may [already have a signed corporate CLA](XXX) for the project in question; administrative details for each of these varies, so please ask if you're at all uncertain whether your potential contributions are covered.
