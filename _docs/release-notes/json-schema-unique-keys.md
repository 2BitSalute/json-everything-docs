# 2.1.0 ([#326](https://github.com/gregsdennis/json-everything/pull/326) / [#366](https://github.com/gregsdennis/json-everything/pull/366) / [#389](https://github.com/gregsdennis/json-everything/pull/389)) {#release-schemauniquekeys-2.1.0}

Updated to use JsonSchema.Net v4.

[#288](https://github.com/gregsdennis/json-everything/issues/288) - `duplicates` token in error message was not being replaced with the appropriate value.  Thanks to [@szilajka](https://github.com/szilajka) for finding and reporting this.

Added `JsonSchemaExtensions.GetUniqueKeys()` to enable easy access to `uniqueKeys` keyword.

# 2.0.1 (no PR)

[#288](https://github.com/gregsdennis/json-everything/issues/288) - Just bumping version to pick up the latest Json.More.Net by default.  This package pull Json.More.Net transitively via JsonPointer.Net which wasn't updated with the move to `JsonNode`.

# [2.0.0](https://github.com/gregsdennis/json-everything/pull/280) {#release-schemauniquekeys-2.0.0}

Updated all functionality to use `JsonNode` instead of `JsonElement` in line with JsonSchema.Net v3.

# [1.1.0](https://github.com/gregsdennis/json-everything/pull/249) {#release-schemauniquekeys-1.1.0}

[#243](https://github.com/gregsdennis/json-everything/pull/243) - Updated System.Text.Json to version 6.

[#190](https://github.com/gregsdennis/json-everything/issues/190) - Added support for custom and localized error messages.

# [1.0.2](https://github.com/gregsdennis/json-everything/pull/200) {#release-schemauniquekeys-1.0.2}

Updated JsonSchema.Net reference to v2.0.0.  Please see [release notes](./json-schema.md) for that library as it contains breaking changes.

# [1.0.1](https://github.com/gregsdennis/json-everything/pull/182) {#release-schemauniquekeys-1.0.1}

Updated JsonPointer.Net to v2.0.0.  Please see [release notes](./json-pointer.md) for that library as it contains breaking changes.

# [1.0.0](https://github.com/gregsdennis/json-everything/pull/120) {#release-schemauniquekeys-1.0.0}

Initial release.