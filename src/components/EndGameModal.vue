<template>
  <div class="dimmer" @click="closeModal">
    <transition name="window" appear>
      <div class="window">
        <h1>
          <slot />
        </h1>
        <button @click="closeModal">Close</button>
      </div>
    </transition>
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
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.3);
  z-index: 1;

  display: flex;
  justify-content: center;
  align-items: center;
}

.window {
  width: 21rem;
  height: 9rem;
  background-color: #696969;
  border-radius: 5px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  flex-direction: column;
  z-index: 10;
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

.window-enter {
  opacity: 0;
  transform: scale(0);
}

.window-enter-active {
  transition: all 0.4s cubic-bezier(0.97, 0.52, 0.25, 1.32);
  transition-delay: 0.2s;
}
</style>

