# Worlds most EVIL keyword

---

# Pieter Joost van de Sande

---

# Software Architect

![fit](concepts/blueprint.png)

---

![inline fit](logos/wercker.png)

[.background-color: #FFFFFF]

^ wercker
* continues integration and delivery platform
* serving 100k builds per day
* world wide customer base
* sold to Oracle

---

![inline fit](logos/doubledutch.png)

^ doubledutch
* eventing platform software
* 10.000 of people simultaneously try to slam the API
* we serve millions of customers a year
* There’s our data pipeline that processes billions of data points. We capture taps, swipes, and scrolls and report on them in real time.

---

![inline fit](logos/microsoft.png)

[.background-color: #FFFFFF]
 
^ microsoft
* CQRS Journey advisory board

---

![inline fit](logos/apple.png)

[.background-color: #FFFFFF]

^ apple
* FoundationDB Go bindings
* distributed acid complianed database that is internet scale

---

![fit](logos/happypancake.jpeg)

[.background-color: #FF9F00]

^ HappyPancake
* largest dating site in Sweden, Norway and Finland
* average session 18 minutes

---

# 155 day streak record

![](logos/github.png)

[.background-color: #FFFFFF]

---

![fit](logos/vim.png)

---

![inline fit](concepts/exit-vim.png)

[.background-color: #000000]

---

![fit](concepts/i3.png)

[.background-color: #000000]

---

![inline](concepts/streamsdb.png)

---

# I :heart: messaging AND distribution

![fit](concepts/distributed-system.png)


[.text: #000000 ]
[.background-color: #FFFFFF]

---

![](concepts/talk.png)

[.background-color: #000000]

^
* What will we talk about?
* Messaging, scalability, domain driven design, micro services

---

# Let's talk about EVIL

![fit](concepts/evil.jpg)

---

# [fit] What's the most evil
# [fit] keyword you know?

---

![inline fit](concepts/goto.png)

___

# IF is EVIL

![fit](concepts/bloody-if.png)

---

# Two-face

![fit](concepts/two-face.jpg)

---

# the protector

![left fit](concepts/protector.png)

---

```go
// name is required
if len(name) == 0 {
  return errors.New("name is required")
}

// make sure there is a product
if product == nil {
  return errors.New("product cannot be nil")
}

// never go out of bound
if cursor > limit {
  return errors.New("cursor out of bound")
}

// make sure is valid json
if ok, err := deserialize(input); !ok {
  return errors.Wrap(err, "deserialization failed")
}
```

^ 
* protects our code for all the evil from the outside world
* make sure something is not null
* index is not out of bound
* good old defensive programming

---

# the deceiver

![right fit](concepts/deceiver.png)

^ 
* IF here is not technocratic, but business oriented

---

![](concepts/monolith.png)

A monolithic design is characterised by such a tight coupling among modules that they have no independent existence.

---

![inline fit](concepts/boom.png)

[.background-color: #FFFFFF]

---

![left fit](concepts/checkout.png)

# checkout policy

the product must be in *stock*

it must be allowed to *ship* to country

the *payment* must be accepted

*product* must not be deleted

the *price* should not have changed

^ TODO: are the business rules still in line with the story?

---

# how would we satisfy the requirements?

![inline](concepts/checkout-call.png)

[.background-color: #FFFFFF]

^ TODO: break down in steps

---

```golang
func Checkout(r CheckoutRequest) {
  // make sure we have stock
  if inventory.GetStock(r.ProductId) < r.Quantity {
    return errors.New("insufficient stock")
  }

  // make sure it is not deleted
  if product.Status(r.ProductId) == Deleted {
    return errors.New("product unavailable")
  }

  // check the price
  if pricing.GetPrice(r.ProductId) != r.Price {
    return errors.New("product price changed")
  }

  // payment must be accepted
  if payment.GetStatus(r.OrderId) != Accepted {
    return errors.New("payment not accepted")
  }

  // remove quantity from inventory
  inventory.DecreaseStock(r.ProductId, r,Quantity)

  // ship it!
  shipment.MarkReady(r.OrderId);
}
```

^ here the if statement is not technocratic, but a business rule

---

![inline fit](concepts/service-calling.png)

[.background-color: #FFFFFF]

---

# How do we guarantee consistency?

| ID | Name      | Quantity | Price  |
|----|-----------|----------|--------|
| 1  | Product A | 13       | € 42,- |
| 2  | Product B | 1        | € 3,14 |

---

```pgsql
begin transaction
  var stock = SELECT stock FROM Products
              WHERE Id = @ProductID;

  IF(stock <= @Quantity)
    THROW 51000, 'insufficient stock';  

  var payed = SELECT isPayed FROM Order
              WHERE Id = @PaymentID;

  IF(payed)
    THROW 52000, 'order not payed';
  IF(kk)

  UPDATE Products SET Stock = stock-@Quantity;

  UPDATE Order SET readyToShip = true;
end transaction
```

^ // serializable isolation level

---

# How to *scale*?

![](concepts/scale-mini.jpg)
![](concepts/small-scale.jpg)
![](concepts/large-scale.jpg)

^
* scale with bigger hardware
* horizontal scaling
* forklift upgrade
* won't work with document databases

---

![fit](concepts/database-cluster.png)

^
* we can introduce more nodes
* data will be replicated
* but won't be able to have ACID properties anymore

---

![fit](concepts/document-database.png)

^ trully internet scale, but lack of transactions

---

![](concepts/coupling.png)

# coupling

^ coupling is the degree to which each module relies on each one of the other modules.

---

# Split it!

![](concepts/breaking-monolith.jpeg)

---

# Micro Services

![inline](concepts/microservices.png)

[.background-color: #FFFFFF]

---

![fit](concepts/autonomous.png)

^ the most important property of an micro service

[.background-color: #FFFFFF]

---

> “No μService should ever depend on another being available"

---

![fit](concepts/temporal-coupling.png)

[.background-color: #FFFFFF]

---

![inline fit](concepts/service-calling.png)

[.background-color: #FFFFFF]

---

# How do you depend on your partner?

![](concepts/wedding.jpg)

---

![fit](concepts/whatsapp.jpg)

[.background-color: #000000]

---

![fit](concepts/queue.png)

[.background-color: #FFFFFF]

---

![inline fit](concepts/order-chat.png)

---

![](concepts/checkout-step-1.png)

---

![](concepts/checkout-step-2.png)

---

![](concepts/checkout-step-3.png)

---

![](concepts/checkout-step-4.png)

---

![](concepts/checkout-step-5.png)

---

![](concepts/checkout-process.png)

^
* what happens when during checkout the price changes?

---

# Add Product to Shopping cart

```js
{
  orderID: 1,
  productID: 2,
  quantity: 1,
  price: {
    amount: 7.98,    // <-- save price in message
    currency: "USD", 
  }
}
```

---

![](concepts/vertical-decomposition.png)

---

![fit](concepts/composite-ui.png)

[.background-color: #FFFFFF]

---

# compensating actions

![inline](concepts/undo.png)

[.background-color: #FFFFFF]

---

![left fit](concepts/dating-profile.png)
![right fit](concepts/hpc-services.png)

[.background-color: #FFFFFF]

---

> No μService represents an *business* entity, but rather serves an *expectation* of one

---

# You've been a great audience!

Questions?

WhatsApp: +31 6 54 22 44 89
Mail: pj@craftify.nl
Twitter: @pjvds
