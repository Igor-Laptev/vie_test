<template>
  <div>
    <input
      type="text"
      v-model="startQuery"
      @keyup.enter="searchRoute"
      placeholder="Начальный адрес"
    />
    <input
      type="text"
      v-model="endQuery"
      @keyup.enter="searchRoute"
      placeholder="Конечный адрес"
    />
    <button @click="searchRoute">Построить маршрут</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        startQuery: '',
        endQuery: '',
      };
    },
    methods: {
      searchRoute() {
        if (!this.startQuery.trim() || !this.endQuery.trim()) return;

        ymaps.geocode(this.startQuery).then((res) => {
          const startCoords = res.geoObjects.get(0).geometry.getCoordinates();

          ymaps.geocode(this.endQuery).then((res) => {
            const endCoords = res.geoObjects.get(0).geometry.getCoordinates();
            this.$emit('routeFound', { startCoords, endCoords });
          });
        });
      },
    },
  };
</script>
