<template>
  <div id="app">
    <div v-if="elements">
      <p>{{ displayNode }}</p>
      <template v-for="elem in displayChildren">
        <button v-if="elem.children" :key="elem.node" @click="ask(elem)">
          {{ elem.node }}
        </button>
        <p v-else :key="elem.node">{{ elem.node }}</p>
      </template>
      <button @click="restart">restart</button>
    </div>
    <div v-else>
      <button v-for="part in parts" @click="start(part)" :key="part">
        {{ part }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function () {
    return {
      elements: null,
      displayNode: {},
      displayChildren: [],
      parts: ["cephale", "cuisse", "epaule", "genou", "hanche"],
    };
  },
  methods: {
    ask: function (question) {
      this.displayNode = question.node;
      this.displayChildren = question.children;
    },
    start: function (part) {
      fetch(`./datas/${part}.json`, {
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
      })
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.elements = data;
          this.displayNode = this.elements.node;
          this.displayChildren = this.elements.children;
        });
    },
    restart: function () {
      this.elements = null;
    },
  },
};
</script>
