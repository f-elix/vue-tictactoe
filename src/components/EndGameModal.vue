<template>
  <div class="dimmer" @click="closeModal">
    <div class="window">
      <h1>
        <slot />
      </h1>
      <button @click="closeModal">Close</button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    closeModal() {
      this.$emit("close-modal");
    }
  }
};
</script>

<style scoped>
.dimmer {
  position: fixed;
  height: 100vh;
  width: 100vh;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  z-index: 1;
}

.window {
  margin-top: 6rem;
  width: 21rem;
  height: 9rem;
  background-color: #696969;
  border-radius: 5px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  flex-direction: column;
  z-index: 2;
}

.window h1 {
  font-weight: bold;
}

.window button {
  padding: 0.75rem;
  border: none;
  border-radius: 5px;
  font-weight: bold;
  background-color: var(--white);
}
</style>

