# GRPC

## Types

|Protobuf Type| C# type|
|--|--|
|double | double |
|float | float |
|int32 | int |
|int64 | long |
|uint32 | uint |
|uint64 | ulong |
|sint32 | int |
|sint64 | long |
|fixed32 | uint |
|fixed64 | ulong |
|sfixed32 | int |
|sfixed64 | long |
|bool | bool |
|string | string |
|bytes | ByteString |

## DateTimes

``` proto
import "google/protobuf/duration.proto"; 
import "google/protobuf/timestamp.proto";
````

|C# type|Proto WK Type|
|--|--|
| DateTimeOffset | google.protobuf.Timestamp |
| DateTime | google.protobuf.Timestamp |
| TimeSpan | google.protobuf.Duration|

## Nullable Types

|C# Type| Well Known Type Wrapper|
|--|--|
| double? | google.protobuf.DoubleValue |
| float? | google.protobuf.FloatValue |
| int? | google.protobuf.Int32Value |
| long? | google.protobuf.Int64Value |
| uint? | google.protobuf.UInt32Value |
| ulong? | google.protobuf.UInt64Value|