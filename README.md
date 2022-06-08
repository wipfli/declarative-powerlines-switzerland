# declarative-powerlines-switzerland
Map of powerlines in Switzerland built with Planetiler's declarative schema approach and PMTiles

This is the same map as https://github.com/wipfli/powerlines-switzerland but this time we use Planetiler's declarative schema approach.

[Demo](https://wipfli.github.io/declarative-powerlines-switzerland/)

<a href="https://wipfli.github.io/declarative-powerlines-switzerland">
    <img src="demo.png" />
</a>

## Generate Tiles

Open `power.yml` and fill it with:

```yml
schema_name: Power
schema_description: Features that represent electrical power grid
attribution: <a href="https://www.openstreetmap.org/copyright" target="_blank">&copy;
  OpenStreetMap contributors</a>
sources:
  osm:
    type: osm
    url: geofabrik:switzerland
layers:
- name: power
  features:
  - sources:
    - osm
    geometry: line
    min_zoom: 6
    include_when:
      power:
      - line
    attributes:
    - key: power
```

Clone Planetiler

```
git clone git@github.com:onthegomap/planetiler.git
```

Install Java 16+, see https://github.com/onthegomap/planetiler/blob/main/CONTRIBUTING.md

Compile Planetiler:

```
/mvnw clean package
```

Run Planetiler:

```
java -jar planetiler-dist/target/*-with-deps.jar generate-custom --schema=power.yml --download
```

Install PMTiles python package:

```
pip3 install pmtiles
```

Convert `data/output.mbtiles` to `data/output.pmtiles`:

```
pmtiles-convert data/output.mbtiles data/output.pmtiles
```
