<template lang="pug">
#app
  h1 Notes
  section.toolbar
    input(placeholder="filename...")
    button Save
    button Load
  section.list
    .item(v-for="item in items")
      .actions
        button(@click="up(item)") Up
        button(@click="down(item)") Down
        button(@click="del(item)") Delete
      //- TODO: bind item.text to data
      .content(contenteditable="true") {{item.text}}
  section.toolbar
    button(@click="add") Add new
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          text: "foo"
        }
      ]
    };
  },
  methods: {
    add() {
      this.items.push({
        text: ""
      });
    },
    up(item) {
      const i = this.items.indexOf(item);
      if (i === 0) {
        return;
      }
      const temp = this.items[i - 1];
      this.items[i - 1] = item;
      this.items[i] = temp;
      this.$forceUpdate()
    },
    down(item) {
      const i = this.items.indexOf(item);
      if (i === this.items.length - 1) {
        return;
      }
      const temp = this.items[i + 1];
      this.items[i + 1] = item;
      this.items[i] = temp;
      this.$forceUpdate()
    },
    del(item) {
      this.items.splice(this.items.indexOf(item), 1);
    }
  }
};
</script>

<style lang="stylus">
primary-color = #ffca3a;
text-color = hsl(0, 0, 25);

#app {
  margin: 3rem;
  color: text-color;
}

common() {
  border: unset;
  padding: calc(0.5em);
  background: unset;
  outline: solid 2px primary-color;
  outline-offset: -2px;
  transition: 0.1s all ease-out;

  &:hover, &:focus {
    outline: solid 6px primary-color;
    outline-offset: -6px;
  }
}

input, textarea, .toolbar button {
  common();
}

textarea {
  width: 100%;
  height: fit-content;
  min-height: fit-content;
  resize: vertical;
}

.toolbar button, input {
  margin-right: 1rem;
}

.toolbar {
  margin-top: 1rem;
  margin-bottom: 1rem;
}

.item {
  button {
    display: block;
    border: unset;
    background: unset;
    margin: 0;
    padding: 0;
    color: unset;
  }

  margin-top: 1rem;
  margin-bottom: 1rem;
  border-left: solid 2px primary-color;
  padding-left: 0.2rem;
  padding-bottom: 0.2rem;
  display: grid;
  grid-template-columns: auto 1fr;

  > * {
    padding: 0.2rem;
  }
}
</style>
