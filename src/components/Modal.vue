<script setup>
import { defineProps, ref, isVNode, onMounted } from 'vue';
import SvgClose from "@/components/icons/SvgClose.vue";

const props = defineProps({
  isOpen: {
    type: Boolean,
    required: true,
  },
  onClose: {
    type: Function,
    required: true,
  },
  title: {
    type: String,
  },
  content: {
    validator(value, props) {
      return () => {
        if (!props.isOpen) {
          // Если модальное окно закрыто, то валидация не нужна
          return true
        } else {
          return (isVNode(value) || typeof value === 'string')
        }
      }
    },
    required: true,
  },
});

function handleClose() {
  props.onClose();
}

</script>

<template>
  <div v-if="isOpen" class="modal-overlay" @click.self="handleClose">
    <div class="modal">
      <div class="header">
        <SvgClose class="close-icon" size="26" color="#58595B" @click="handleClose"/>
      </div>
      <!--   Обязательно ли title через props прокидывать?   -->
      <!--   <div class="main-title">{{ title }}</div>       -->
      <div v-if="typeof content === 'string'">{{ content }}</div>
      <component v-else :is="content"/>
    </div>
  </div>
</template>

<style scoped>
.header {
  right: 0;
  margin: 10px;
  display: flex;
  justify-content: right;
}

.close-icon {
  cursor: pointer;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  display: flex;
  flex-direction: column;
  width: 350px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}
</style>