<template>
  <div>
    <Search @routeFound="buildRoute" />
    <div id="map" style="width: 100%; height: 500px"></div>
  </div>
</template>

<script>
  import Search from '../Lib/Search.vue';

  export default {
    components: {
      Search,
    },
    data() {
      return {
        map: null,
        placemark: null,
        route: null,
      };
    },
    mounted() {
      ymaps.ready(this.initMap);
    },
    methods: {
      initMap() {
        this.map = new ymaps.Map('map', {
          center: [59.9342802, 30.3350986], // Санкт-Петербург
          zoom: 10,
        });

        this.setMarker([59.9342802, 30.3350986]); // Устанавливаем метку на СПб
      },
      setMarker(coords) {
        if (this.placemark) {
          this.map.geoObjects.remove(this.placemark);
        }

        this.placemark = new ymaps.Placemark(coords, {
          balloonContent: 'Найденный адрес',
        });

        this.map.geoObjects.add(this.placemark);
        this.map.setCenter(coords, 12);
      },
      buildRoute({ startCoords, endCoords }) {
        if (this.route) {
          this.map.geoObjects.remove(this.route);
        }

        ymaps.route([startCoords, endCoords]).then((route) => {
          this.route = route;
          this.map.geoObjects.add(route);
          this.map.setBounds(route.getBounds());
        });
      },
    },
  };
</script>
