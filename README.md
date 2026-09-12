# BME280 sensor API

### Sensor overview

BME280 is a combined digital humidity, pressure and temperature sensor based on proven sensing principles.
Its small dimensions and its low power consumption allow the implementation in battery driven devices such as
handsets, GPS modules or watches.

### Target Application
- Context awareness, e.g.skin detection, room change detection.
- Fitness monitoring / well-being
- Home automation control
- Internet of things
- GPS enhancement(e.g.time-to-first-fix improvement, dead reckoning, slope detection)
- Indoor navigation(change of floor detection, elevation detection)
- Outdoor navigation, leisure and sports applications
- Weather forecast
- Vertical velocity indication(rise/sink speed)

### Feature
- Pressure
- Temperature
- Humidity

### Important links

- [BME280 product page](https://www.bosch-sensortec.com/products/environmental-sensors/humidity-sensors-bme280/)
- [BME280 datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bme280-ds002.pdf)
- [BME280 shuttle board flyer](https://www.bosch-sensortec.com/media/boschsensortec/downloads/shuttle_board_flyer/application_board_3_1/bst-bme280-sf000.pdf)
- [Community support page](https://community.bosch-sensortec.com)

## Cura Agrorum fork

Upstream base: `c90d419492e26dd95586598a794e65eb2760753a`.
This fork separates the NVM polling error fix from the project compensation
adaptation. In the double compensation path, temperature and pressure are not
clipped to the sensor operating range. An undefined pressure calculation returns
NaN instead of a fabricated minimum pressure. Humidity retains Bosch's specified
0..100 percent compensation saturation. Integer compensation paths are unchanged;
Cura Agrorum explicitly selects double compensation and checks representability.

Driver and ESP-IDF adapter regressions live in Cura Agrorum under
`firmware/tests/host`, with their build/dependency relationship described in
`firmware/components/bosch_bme280/README.md`. Driver tests compile this actual
source with scripted transport/delay callbacks; adapter tests compile the real
adapter and driver against ESP-IDF/time boundary fakes. Both use the same full
fork commit pin as firmware. Run `CCACHE_DISABLE=1 make test-host` in Cura Agrorum.
This fork has no separate unit-test Makefile or dependency on ESP-IDF.
A standalone reproducer is deferred until the first upstream bug report is
prepared. No upstream report or submission is implied by these local changes.
