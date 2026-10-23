# актори та їхні цілі
Актор 1: Покупець 
    Ціль: вибрати товари та оплатити їх
Актор 2: Платіжний шлюз
    Ціль: отримати запит про оплату та обробити його

# Use Case діаграма за допомогою Mermaid
```mermaid
graph LR
    Customer((Покупець))
    Gateway((Платіжний шлюз))

    UC1([Оформити замовлення])
    UC2([Провести оплату])

    Customer --> UC1
    UC1 -. "<<include>>" .-> UC2
    UC2 --- Gateway
    
    classDef actor fill:#f9f,stroke:#333,stroke-width:2px, color:#000;
    class Customer,Gateway actor;
```
    
