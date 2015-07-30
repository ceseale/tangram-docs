*This is the technical documentation for the "layers" block in Tangram's scene file. For a conceptual overview of the way Tangram applies styles to data, see the [Filters Overview](../overview/filters.md) and the [Styles Overview](../overview/styles.md).*

####`layers`
The `layers` element is a required top-level element in the [scene file](../overview/scene-file.md). It has only one kind of sub-element: a *layer name*, which defines individual layers with a layer filter.

```yaml
layers:
    earth:
        ...
```

### layer name
Required _string_. Can be anything. No default.

```yaml
layers:
    landuse:
        ...
```
## layer parameters

####`data`
Required parameter. Defines the beginning of a [data block](#data-parameters). Usable by top-level layers only. No default.
```yaml
layers:
    landuse:
        data: { source: osm }
```

####`filter`
Optional _object_ or _function_. No default.

A `filter` element may be included once in any layer or sublayer. Only features matching the filter will be included in that layer (and its sublayers). For more on the filtering system, see [Filters Overview](../overview/filters.md).

```yaml
layers:
    roads:
        data: { source: osm }
        filter: { kind: highway }
```

####`visible`
Optional _Boolean_. Allows layer to be turned off and on. Default is `true`.

```yaml
layers:
    landuse:
        data: { source: osm }
        visible: false
```

####`draw`
Required parameter. Defines the beginning of a [draw block](#draw-parameters). For draw parameters, see the [draw](draw.md) entry.
```yaml
layers:
    landuse:
        data: { source: osm }
        draw:
            ...
```

####sublayer name
Optional _string_. Can be anything except the other sublayer parameter: "draw", "filter", and "properties". No default.

Defines a _sublayer_. Sublayers can have all `layer` parameters except `data`, and can be nested. `draw` and `filter` definitions are inherited, and match simultaneously – see the [Filters Overview](../overview/filters.md).

```yaml
layers:
    landuse:
        data: { source: osm }
        filter: ...
        draw: ...
        sublayer:
            filter: ...
            draw: ...
        sublayer2:
            filter: ...
            draw: ...
            subsublayer:
                filter: ...
                draw: ...
```

#### `properties`
Optional parameter. Defines the beginning of a _properties block_. The `properties` block has one kind of sub-element: _custom properties_, which are a key-value pair. Keys can be any string.

Custom properties may be defined here for use in filter and shader effects. These properties may be accessed through the JavaScript API, or through filter and style functions with the `properties` keyword, as a convenient way to allow interactivity.

```yaml
buildings:
    properties:
        min-height: 30
        color: [.3, .3, .5]
    draw:
        polygons:
            filter: function() { return feature.height > properties.min-height; }
            color: function () { return properties.color; }
```


## `data` parameters

####`source`
Required _string_, naming one of the sources defined in the [sources](sources.md) block.

```yaml
data:
    source: osm
```

####`layer`
Optional _string_, naming a top-level named object in the source datalayer. In GeoJSON, this is a _FeatureCollection_. If a `layer` is not specified, the _layer name_ will be used.
```yaml
data:
    source: osm
    layer: buildings
```
The above `layer` refers to the below object:
```json
{"buildings":
    {"type":"FeatureCollection","features":[
        {"geometry":"..."}
    ]}
}
```
Because the _layer name_ is the same as the name of the GeoJSON object, the `data` object's `layer` parameter can be omitted.


## `draw` parameters

See the [draw](draw.md) entry.
