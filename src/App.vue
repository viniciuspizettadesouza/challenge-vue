<template>
  <div>
    <ListBox
        :items="items"
        @selected-items="(items) => (selectedItems = items)"
    />
    <div className="container">
      <h3>emmited items: {{ selectedItems }}</h3>
    </div>
  </div>
</template>

<script>
import ListBox from "./components/ListBox.vue";

export default {
  name: "App",

  components: {
    ListBox,
  },

  data () {
    return {
      selectedItems: [],
    };
  },

  computed: {
    items () {
      return [
        {
          id: 1,
          name: "Sergio Perez",
        },
        {
          id: 2,
          name: "Max Verstappen",
        },
        {
          id: 3,
          name: "Lewis Hamilton",
        },
        {
          id: 4,
          name: "Lando Norris",
        },
        {
          id: 5,
          name: "Daniel Ricciardo",
        },
      ];
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>