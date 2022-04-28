<template>
  <div class="btn-toolbar">
    <div class="btn-group">
      <button type="button" class="btn" data-original-title="Bold - Ctrl+B">
        <i class="fa-solid fa-bold"></i>
        <font-awesome-icon icon="fa-solid fa-user-secret" />
      </button>
      <button
        type="button"
        class="btn btn-light"
        data-original-title="Italic - Ctrl+I"
      >
        <i class="icon-italic"></i>
      </button>
      <button type="button" class="btn btn-light" data-original-title="List">
        <i class="icon-list"></i>
      </button>
      <button type="button" class="btn btn-light" data-original-title="Img">
        <i class="icon-picture"></i>
      </button>
      <button type="button" class="btn btn-light" data-original-title="URL">
        <i class="icon-arrow-right"></i>
      </button>
    </div>
    <div class="btn-group">
      <button
        type="button"
        class="btn btn-light"
        data-original-title="Align Right"
      >
        <i class="icon-align-right"></i>
      </button>
      <button
        type="button"
        class="btn btn-light"
        data-original-title="Align Center"
      >
        <i class="icon-align-center"></i>
      </button>
      <button
        type="button"
        class="btn btn-light"
        data-original-title="Align Left"
      >
        <i class="icon-align-left"></i>
      </button>
    </div>
    <div class="btn-group">
      <button type="button" class="btn btn-light" data-original-title="Preview">
        <i class="icon-eye-open"></i>
      </button>
      <button type="button" class="btn btn-light" data-original-title="Save">
        <i class="icon-ok"></i>
      </button>
      <button type="button" class="btn btn-light" data-original-title="Cancel">
        <i class="icon-trash"></i>
      </button>
    </div>
  </div>
  <div
    contenteditable
    @input="handleInput"
    :dir="dir"
    :style="`text-align:` + align"
    class="text-editor"
  />
  <div
    ref="editor"
    style="
      position: absolute;
      top: 100px;
      left: 500px;
      width: 30px;
      height: 30px;
      background-color: red;
    "
  ></div>
  <button
    id="testbtn"
    style="width: 100px; height: 30px;"
    @click="$refs.editor.style.top = '50px'"
  >
    Test
  </button>
</template>
<script>
const exec = (command, value) => document.execCommand(command, false, value);

// const queryCommandValue = (command) => document.queryCommandValue(command);

export default {
  props: {
    value: { type: String, default: "" },
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
.btn-toolbar {
  width: 70%;
  margin: 0 auto;
}

.text-editor {
  border: 1px solid #aaa;
  text-shadow: 1px 1px 5px #494949;
  height: 400px;
  width: 70%;
  margin: 0 auto;
  padding: 5px;
  outline: none;
}
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
</style>
