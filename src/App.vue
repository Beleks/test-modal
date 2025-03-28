<script setup>
import Modal from "@/components/Modal.vue"
import { ref, h, computed } from 'vue'

const reasons = [
  { id: 1, label: 'Не подходит дата или время' },
  { id: 2, label: 'Не подходит тоннаж груза' },
  { id: 3, label: 'Не подходит объем груза' },
  { id: 4, label: 'Не подходит тип фургона' },
  { id: 5, label: 'Не подходит объем груза' },
  { id: 6, label: 'Нет пропуска в МКАД ТТК СК' },
  { id: 7, label: 'Поломка машины/авария' },
  { id: 8, label: 'Заболел' },
  { id: 9, label: 'Нет санобработки' },
  { id: 10, label: 'Нет мед. книжки' },
  { id: 11, label: 'Нет гидроборта' },
  { id: 12, label: 'Нет растентовки' },
  { id: 13, label: 'Не устраивает ставка за рейс' },
  { id: 14, label: 'Не устраивает маршрут' },
  { id: 15, label: 'Другое' },
]

const orderList = ref([
  {
    orderNumber: 'У00017711',
  },
  {
    orderNumber: 'У00017713',
  },
  {
    orderNumber: 'У00017714',
  }
])


const isModalOpen = ref(false)
const modalTitle = ref('')
const currentOrder = ref({})
const checkedItems = ref(new Set())

function toggleCheckbox(id) {
  if (checkedItems.value.has(id)) {
    checkedItems.value.delete(id)
  } else {
    checkedItems.value.add(id)
  }
}

const modalContent = () => {
  const checkboxes = reasons.map((item, index) => {
    return h('div', { class: 'custom-checkbox', key: item.id, onClick: () => toggleCheckbox(item.id) }, [
      h('div', {
        class: ['checkbox-box', { checked: checkedItems.value.has(item.id) }],
      }),
      h('div', {}, item.label),
    ])
  })

  return h('div', { class: 'modal-content' }, [
    //
    h('div', { class: 'main-title' }, ['Отказаться от выполнения заказа', h('div', { class: 'order-number' }, `№${currentOrder.value.orderNumber}`)]),
    h('div', { class: 'sub-title' }, 'Расскажите, почему отказались от выполнения заказа?'),
    h('div', { class: 'checkbox-list' }, checkboxes),
    h('diu', { class: 'footer' }, [h('div', { class: 'send-button', onClick: () => sendCancelReason() }, 'Отправить')])
  ])
}

function sendCancelReason() {
  if (checkedItems.value.size < 1) {
    console.log('Выберете причину отказа')
    return
  }
  console.log(checkedItems.value, 'Id причин отказа')
  closeModal()
}

function showModal(order) {
  currentOrder.value = order
  isModalOpen.value = true
}

function closeModal() {
  // modalTitle.value = ''
  checkedItems.value.clear()
  modalContent.value = ''
  isModalOpen.value = false
}


</script>

<template>
  <div class="current-orders">
    <div>Текущие заказы:</div>
    <div v-for="order in orderList" class="order-list">
      <div>Номер заказа: {{ order.orderNumber }}</div>
      <div @click="showModal(order)" class="cancel-button">Отказаться</div>
    </div>
  </div>

  <Modal :isOpen="isModalOpen" :title="modalTitle" :onClose="closeModal" :content="modalContent"/>
</template>

<style>
.current-orders {
  width: 600px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin: 20px auto;
}


.modal-content {
  display: flex;
  flex-direction: column;
  /* Для проверки скрола */
  max-height: 500px;
}

.main-title {
  padding: 0 10px;
  text-align: center;
}

.order-number {
  font-weight: bold;
}


.sub-title {
  text-align: center;
  padding: 0 10px;
  margin: 20px 0;
}

.order-list {
  align-items: center;
  display: flex;
  justify-content: space-between;
}

.cancel-button {
  color: white;
  border-radius: 12px;
  padding: 6px;
  font-size: 14px;
  line-height: 24px;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
  background-color: #c53838;
}

.test-button {
  color: black;
  border-radius: 12px;
  padding: 6px;
  font-size: 14px;
  line-height: 24px;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
  background-color: #48c834;
}

.footer {
  background-color: white;
  padding: 10px;
}

.send-button {
  border: 1.5px solid #CD10FF;
  border-radius: 12px;
  padding: 10px;
  font-size: 16px;
  line-height: 24px;
  text-align: center;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.send-button:hover {
  background-color: #CD10FF;
  color: white;
}

.checkbox-list {
  height: 100%;
  padding: 0 10px;
  overflow: auto;
  display: flex;
  flex-direction: column;
}

.custom-checkbox {
  display: flex;
  align-items: center;
  cursor: pointer;
  margin-bottom: 10px;
}

.checkbox-box {
  width: 20px;
  height: 20px;
  border: 2px solid #999;
  border-radius: 5px;
  position: relative;
  margin-right: 10px;
  background-color: white;
}


.checkbox-box.checked::after {
  content: '';
  width: 12px;
  height: 12px;
  background-color: #CD10FF;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 2px;
}
</style>
