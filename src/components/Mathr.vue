<template>
  <span class="root-math" v-bind:class="size" ref="mathJaxEl">{{
    c_formula
  }}</span>
</template>

<script>
export default {
  props: {
    formula: {
      type: String
    },
    safe: {
      type: Boolean,
      default: true
    },
    size: {
      type: String,
      default: "large"
    },
    options: {
      type: Object,
      default: function() {
        return {};
      }
    }
  },
  watch: {
    formula() {
      this.renderMathJax();
    }
  },
  computed: {
    c_formula: function() {
      return `$$${this.formula}$$`;
    }
  },
  mounted() {
    this.renderMathJax();
  },
  methods: {
    renderContent() {
      if (this.safe) {
        this.$refs.mathJaxEl.textContent = this.c_formula;
      } else {
        this.$refs.mathJaxEl.innerHTML = this.c_formula;
      }
    },

    renderMathJax() {
      this.renderContent();
      if (window.MathJax) {
        window.MathJax.Hub.Config({
          tex2jax: {
            inlineMath: [
              ["$", "$"],
              ["(", ")"]
            ],
            displayMath: [
              ["$$", "$$"],
              ["[", "]"]
            ],
            processEscapes: true,
            processEnvironments: true
          },
          // Center justify equations in code and markdown cells. Elsewhere
          // we use CSS to left justify single line equations in code cells.
          displayAlign: "center",
          "HTML-CSS": {
            styles: { ".MathJax_Display": { margin: 0 } },
            linebreaks: { automatic: true }
          },
          ...this.options
        });
        window.MathJax.Hub.Queue([
          "Typeset",
          window.MathJax.Hub,
          this.$refs.mathJaxEl
        ]);
      }
    }
  }
};
</script>
<style scoped lang="sass">

.root-math
  margin: 5px
.large
  font-size: 2em
.small
  font-size: 1em
</style>
