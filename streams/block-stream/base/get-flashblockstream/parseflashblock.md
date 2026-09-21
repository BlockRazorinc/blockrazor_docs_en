---
description: Get the code example to parse FlashBlock stream provided by BlockRazor
metaLinks:
  canonical: parseflashblock.md
  alternates:
    - >-
      https://app.gitbook.com/s/QJcHRn7SY50Ny5UQhXHy/streams/block-stream/base/get-flashblockstream/parseflashblock
---

# Base ParseFlashBlock

The parsing method for FlashBlock is as follows:

```go
// ParseFlashBlockByte decompresses the brotli-compressed binary data of a flash block.
func ParseFlashBlockByte(data []byte) (string, error) {
	br := brotli.NewReader(bytes.NewReader(data))
	var buf bytes.Buffer
	_, err := buf.ReadFrom(br)
	if err != nil {
		return "", err
	}
	return buf.String(), nil
}
```

You can access the example [here](https://github.com/BlockRazorinc/base-api-client-go/blob/1d46c2983420d6da645992a9f3ed51688f7dac88/main.go#L242C1-L242C56).

