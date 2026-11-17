# намір
деталізувати внутрішню взаємодію компонентів системи для кожного сценарію оплати карткою, щоб кожен крок Gherkin відповідав діаграмі та кожен учасник відповідав ER-діаграмі

# критерії прийняття
- коли Customer натискає "Оплата" тоді "PaymentPage" викликає "OrderService.initiatePayment()" і показує статус "У обробці" до отримання результату, а "OrderService" синхронно викликає "PaymentGateway.paymentRequest(orderId, tokenId)"
- коли "PaymentGateway.paymentRequest(orderId, tokenId)" повертає "approved(receiptId)" (сценарій 1) тоді "OrderService": 
1. викликає "OrderDB.savePayment(status = SUCCESS, method = CARD, amount = SUM(unit_price * quantity), paid_at = now(), order_id = orderId)" який зберігає запис про оплату
2. викликає "OrderDB.updateOrderStatus(orderId, PAID)"" що оновлює у записі замовлення статус
3. повертає "PaymentPage" результат про успішну оплату та квитанцію "success(receiptId)"
    - "PaymentPage" показує "Оплачено"
-  коли "PaymentGateway.paymentRequest(orderId, tokenId)" повертає "declined(INSUFFICIENT_FUNDS)" (сценарій 2) тоді "OrderService" повертає "PaymentPage" результат про недостатність коштів. "PaymentPage" показує "Відхилено через недостатність коштів". ORDER.status не змінюється
- коли "PaymentGateway.paymentRequest(orderId, tokenId)" не відповідає 30 секунд (сценарій 3) тоді "OrderService" автоматично перериває транзакцію та повертає "PaymentPage" "paymentFailed(TIMEOUT)". "PaymentPage" показує "Платіжний шлюз не відповідає". ORDER.status не змінюється