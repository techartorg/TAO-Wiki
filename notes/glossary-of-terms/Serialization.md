> It's when you pour more cereal into milk that has already had cereal in it, then pour ever more milk

Serialization is the act of converting various types of data into a stream of bytes, allowing it to be stored or transferred more easily.

There are many different types of serialization for different use cases:

#### Text-based vs binary

Text-based serialization is intended to be easier to inspect. Usually this means encoding text and numbers in their human-readable forms.

#### Direct vs compressed

Serialization can utilize various forms of compression, which generally means one or more of:

- replacing duplicates of frequently repeated data with shorter sequences referring to the data
	- note that many game engines also do this step at the archive file level in the form of LZ-style compression, which would make it redundant to do the same thing at the file level as well
- using [variable-length encodings](https://en.wikipedia.org/wiki/Variable-length_quantity) with the assumption that smaller values will be more common and should therefore take less space

In this context, "direct" simply means that the value was saved as-is, without additional transformations to make it take less space.

#### Ordered vs schema-based vs mapped

Ordered serialization simply writes all elements in the order they were given to the output byte stream. This generally requires knowing how the serialization was done to unserialize the data.

Schema-based serialization formalizes the knowledge in an external definition file, allowing serialization and unserialization to agree on the contents of the data.

Mapped serialization encodes data using key-value maps, with the intention that the elements may be accessed in a different order.

#### Command-stream vs offset-based subcomponents (if structures need to refer to other structures)

Command-stream serialization most commonly defines the start and end (if necessary) of structures, effectively nesting them in the generated bytes. Example: JSON (using `[` to specify the beginning of an array and `]` for the end).

Offset-based serialization instead opts to use references to sub-structures instead of nesting them. This allows skipping the creation of intermediate structures before accessing the data. Example: EXE files, game data archives.

Command-stream serialization generates more compressible data but requires an intermediate step before elements can be accessed randomly. The compressibility comes from frequently repeating byte patterns (e.g. minified JSON frequently repeats the `},{"` sequence) whereas offsets tend to be at least somewhat different.

---

#### Example classification of common formats

* JSON: text-based, direct, mapped, command-stream
- Protobuf: binary, compressed, schema-based, command-stream
- ZIP/TAR: binary, direct, schema-based, command-stream
- Game archive/package files: binary, direct, schema-based, offset-based

