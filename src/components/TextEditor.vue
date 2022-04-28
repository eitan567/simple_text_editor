<template>
  <p
    contenteditable
    @input="handleInput"
    :dir="dir"
    :style="`text-align:` + align"
    class="text-editor"
  ></p>
</template>
<script>
const exec = (command, value) => document.execCommand(command, false, value);

// const queryCommandValue = (command) => document.queryCommandValue(command);

export default {
  props: {
    value: { type: String, default: "<p></p>" },
    dir: String,
    align: String,
  },
  mounted() {
    this.$el.innerHTML = this.value;
  },
  // We need to ensure we update the innerHTML when it changes,
  // without resetting the cursor.
  watch: {
    value(newValue) {
      if (this.$el.innerHTML !== newValue) this.$el.innerHTML = newValue;
    },
  },
  methods: {
    // We emit changes as HTML. Alternatively you could serialise
    // the innerHTML, which might require debouncing the input
    // for performance reasons.
    handleInput(e) {
      const firstChild = e.target;

      if (firstChild && firstChild.nodeType === 3) exec("formatBlock", "<p>");
      else if (this.$el.innerHTML === "<br>") this.$el.innerHTML = "";

      this.$emit("input", this.$el.innerHTML);
    },

    // You could use a handler like this to listen to
    // the `keyup` event in IE.
    // handleDelayedInput(e) {
    //   this.$nextTick(() => this.handleInput(e));
    // },
    // handleKeydown(e) {
    //   if (
    //     e.key.toLowerCase() === "enter" &&
    //     queryCommandValue("formatBlock") === "blockquote"
    //   ) {
    //     this.$nextTick(() => exec("formatBlock", "<p>"));
    //   } else if (e.ctrlKey) {
    //     switch (e.key.toLowerCase()) {
    //       case "b":
    //         e.preventDefault();
    //         this.$nextTick(() => exec("bold"));
    //         break;

    //       case "i":
    //         e.preventDefault();
    //         this.$nextTick(() => exec("italic"));
    //         break;

    //       case "u":
    //         e.preventDefault();
    //         this.$nextTick(() => exec("underline"));
    //         break;

    //       default:
    //         break;
    //     }
    //   }
    // },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.text-editor h3 {
  margin: 40px 0 0;
}
.text-editor ul {
  list-style-type: none;
  padding: 0;
}
.text-editor li {
  display: inline-block;
  margin: 0 10px;
}
.text-editor a {
  color: #42b983;
}

.text-editor p {
  margin: 0;
  padding: 0;
  line-height: 1em;
}

.text-editor {
  outline: none;
  border: 1px solid rgb(110, 110, 110);
  height: 50vh;
  width: 80vh;
  max-width: 80vh;
  margin: 0 auto;
  overflow: auto;
}
</style>
