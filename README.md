# json2xml

Package json2xml converts a JSON structure to XML.

Forked from `https://github.com/MJKWoolnough/json2xml`

json2xml wraps each type within xml tags named after the type. For example:-

An object is wrapped in `<object></object>` An array is wrapped in
`<array></array>` A boolean is wrapped in `<boolean></boolean>` , with either
"true" or "false" as chardata A number is wrapped in `<number></number>` A
string is wrapped in `<string></string>` A null becomes `<null></null>`, with no
### chardata

When a type is a member of an object, the name of the key becomes an attribute
on the type tag, for example: -

```json
{
    "Location": {
    	"Longitude": -1.8262,
    	"Latitude": 51.1789
    }
}
```

...becomes...

```xml
<object>
    <object name="Location">
    	<number name="Longitude">-1.8262</number>
    	<number name="Latitude">51.1789</number>
    </object>
</object>
```

## Usage

```go
var (
	ErrInvalidKey   = errors.New("invalid key type")
	ErrUnknownToken = errors.New("unknown token type")
	ErrInvalidToken = errors.New("invalid token")
)
```
Errors.

#### func  Convert

```go
func Convert(j JSONDecoder, x XMLEncoder) error
```
Convert converts JSON and sends it to the given XML encoder.

#### type Converter

```go
type Converter struct {
}
```

Converter represents the ongoing conversion from JSON to XML.

#### func  Tokens

```go
func NewConverter(j JSONDecoder) *Converter
```
NewConverter provides a JSON converter that implements the xml.TokenReader interface.

#### func (*Converter) Token

```go
func (c *Converter) Token() (xml.Token, error)
```
Token gets a xml.Token from the Converter, as per the xml.TokenReader interface.

#### type JSONDecoder

```go
type JSONDecoder interface {
	Token() (json.Token, error)
}
```

JSONDecoder represents a type that gives out JSON tokens, usually implemented by
*json.Decoder It is encouraged for implementers of this interface to output
numbers using the json.Number type, as it reduces needless conversions. Users of
the json.Decoder implementation should call the UseNumber method to achieve this.

#### type XMLEncoder

```go
type XMLEncoder interface {
	EncodeToken(xml.Token) error
}
```

XMLEncoder represents a type that takes XML tokens, usually implemented by
*xml.Encoder.