# намір
побудова концептуальної ER-моделі онлайн-магазину техніки, яка включає в себе продаж товарів, оформлення замовлення з декількома товарами та оплату замовлення частинами.

# сутності та атрибути
CUSTOMER: 
id: int PK
name: string
email: string

ITEM:
id: int PK
name: string
brand_id: int FK
description: string
price: numeric
status: string

ORDER:
id: int PK
status: string
customer_id: int FK

ORDER_ITEM:
id: int PK
quantity: int
order_id: int FK
item_id: int FK
unit_price: numeric

PAYMENT:
id: int PK
order_id: int FK
method: string
amount: numeric
paid_at: datetime
status: string

BRAND:
id: int PK
name: string

# зв'язки та кардинальності
CUSTOMER 1:N ORDER
BRAND 1:N ITEM
ITEM 1:N ORDER_ITEM
ORDER 1:N ORDER_ITEM
ORDER 1:N PAYMENT

Між ORDER та ITEM зв'язок M:N. Він реалізується через асоціативну сутність ORDER_ITEM бо сам зв'язок має власні атрибути такі як quantity, unit_price.

# критерії прийняття
- зв'язки мають явну кардинальність 
- зв'язок M:N реалізований через асоціативну сутність ORDER_ITEM
- нормалізація виконана за 3NF: дані в таблиці залежать від основного ключа, неключові атрибути не залежать від інших неключових
- кожна сутність має PK
- всі PK мають тип int
- у всіх FK тип збігається з відповідним PK
- узгоджені типи ідентифікаторів
- усі атрибути пов'язані з грошима мають тип numeric
- назви полів у spec та на діаграмі збігаються
- немає проміжних сутностей без власних атрибутів
- ITEM.price зберігає поточну ціну в каталозі; ORDER_ITEM.unit_price зберігає ціну на момент замовлення
- у paid_at тип даних - datetime, бо в один день може бути декілька замовлень 
