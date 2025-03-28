#  Компонент Modal.vue

Компонент модального окна открывающийся в центре страницы.
При нажатии на область вне модального окна или на кнопку закрытия, модальное окно закрывается.

Использование
```Vue
<template>
  <div class="main-page">
    Какой-то контент...
  </div>

  <Modal 
    :isOpen="isModalOpen"
    :title="modalTitle"
    :onClose="closeModal"
    :content="modalContent"
  />
</template>
```

Входные параметры
```JS
isOpen: {
  // Определяет открыто ли окно
  type: Boolean,
  required: true,
}

onClose: {
  // Функция, вызываемая для закрытия модального окна
  type: Function,
  required: true
}

title: {
  // Заголовок модального окна
  type: String,
  required: true
}

content: {
  // Содержимое модального окна
  type: [VNode, String], 
  required: true
}
```

Комментарии к заданию:
* Реализован сброс данных которые используются в модальном окне
* Вывод в консоль id причин отказа при нажатии на кнопку "Отправить"
* Если ни одна причина не выбрана то при клике на "отправить" ничего не произойдёт


## Установка и запуск

```
npm install
```

```
npm run dev
```

```
npm run build
```
