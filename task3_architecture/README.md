# Петрушка Зеленая — Архитектура PUSH-уведомлений

Верхнеуровневая архитектура системы отправки PUSH-уведомлений для мобильного приложения интернет-магазина.

## Общая схема

![Architecture](/high-level-diagram.svg)

## Компоненты

| Компонент | Описание |
|-----------|----------|
| **Push Service** | Регистрация токенов, маршрутизация уведомлений |
| **RabbitMQ** | Очереди с приоритетами: critical, high, regular, scheduled |
| **Workers** | Обработчики очередей разной срочности |
| **FCM/APNs** | Провайдеры доставки push |

## Потоки данных

### Abandoned Cart
![Abandoned Cart](/data-flow-abandoned-cart.svg)

### Order Cancelled
![Order Cancelled](/data-flow-order-cancelled.svg)

## Типы уведомлений

| Тип | Триггер | Очередь | Задержка |
|-----|---------|---------|----------|
| Отмена заказа | Статус cancelled | Critical | 0 |
| Abandoned cart | Корзина неактивна 24ч | Scheduled | 24ч |
| Реклама | Ручной запуск | Regular | по расписанию |
