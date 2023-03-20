<template>
  <div id="wrapper">
    <div id="output" v-html="renderedHTML"></div>
    <div id="code-container">
      <textarea
        id="html-text"
        placeholder="Add HTML here"
        rows="20"
        cols="50"
        v-model="htmlText"
      ></textarea>
      <textarea
        id="css-text"
        placeholder="Add CSS here"
        rows="20"
        cols="50"
        v-model="cssText"
        @input="updateStyle"
      ></textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: "viewer",
  data() {
    return {
      htmlText: "",
      cssText: "",
    };
  },
  methods: {
    updateStyle() {
      const styleEl = document.getElementById("playground-style");
      if (styleEl) {
        styleEl.innerHTML = this.cssText;
      } else {
        const newStyleEl = document.createElement("style");
        newStyleEl.setAttribute("id", "playground-style");
        newStyleEl.innerHTML = this.cssText;
        document.head.appendChild(newStyleEl);
      }
    },
  },
  computed: {
    renderedHTML() {
      const parser = new DOMParser();
      const doc = parser.parseFromString(this.htmlText, "text/html");
      return doc.body.innerHTML;
    },
  },
};
</script>

<style>
#wrapper {
  height: 100%;
  width: 100%;
}

#code-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

#output {
  margin: 2rem;
}
</style>
