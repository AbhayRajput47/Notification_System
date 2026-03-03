# 📢 Notification System (LLD – Design Patterns Implementation)

## 📌 Overview

This project is a Low-Level Design (LLD) implementation of a Notification System in C++ using multiple design patterns:

- ✅ Decorator Pattern  
- ✅ Observer Pattern  
- ✅ Strategy Pattern  
- ✅ Singleton Pattern  

The system allows sending notifications through multiple channels (Email, SMS, Popup) while dynamically modifying notification content (Timestamp, Signature).

---

# 🏗️ Architecture Overview

System flow:

Client  
→ NotificationService (Singleton)  
→ NotificationObservable  
→ Observers (Logger, NotificationEngine)  
→ Strategies (Email, SMS, Popup)

---

# 🎯 Design Patterns Used

## 1️⃣ Decorator Pattern

Used to dynamically modify notification content without changing the base class.

### Classes:
- `INotification` (Interface)
- `SimpleNotification` (Concrete Component)
- `INotificationDecorator` (Abstract Decorator)
- `TimestampDecorator`
- `SignatureDecorator`

### Example:

```cpp
INotification* notification = new SimpleNotification("Your order has been shipped!");
notification = new TimestampDecorator(notification);
notification = new SignatureDecorator(notification, "Customer Care");
```
---
# 2️⃣ Observer Pattern

The Observer Pattern is used to notify multiple observers whenever a new notification is created.

It ensures loose coupling between the subject (observable) and its observers.

## 📌 Interfaces

- `IObserver`
- `IObservable`

## 📌 Concrete Classes

- `NotificationObservable`
- `Logger`
- `NotificationEngine`

## 🔄 Working

Whenever `setNotification()` is called inside `NotificationObservable`, all registered observers are automatically notified.

```cpp
void setNotification(INotification* notification) {
    if (currentNotification != nullptr) {
        delete currentNotification;
    }
    currentNotification = notification;
    notifyObservers();
}
```
3️⃣ Strategy Pattern
====================

The Strategy Pattern is used to support multiple notification delivery channels without modifying the engine.

This makes the system extensible and follows the Open-Closed Principle (OCP).

📌 Interface
------------

* `INotificationStrategy`

📌 Concrete Strategies
----------------------

* `EmailStrategy`
* `SMSStrategy`
* `PopUpStrategy`

Each strategy defines its own way of sending a notification:

C++

class EmailStrategy : public INotificationStrategy {
public:
    void sendNotification(string content) override {
        cout << "Sending email Notification\n" << content;
    }
};

The `NotificationEngine` simply iterates over all strategies:

C++

for(const auto notificationStrategy : notificationStrategies) {
    notificationStrategy->sendNotification(notificationContent);
}

To add a new channel (e.g., Push Notification), simply create a new strategy class without modifying existing code.

* * *

4️⃣ Singleton Pattern
=====================

`NotificationService` is implemented as a Singleton to ensure:

* Only one notification manager exists
* Centralized control
* Global access point

📌 Implementation
-----------------

C++

static NotificationService* instance;

static NotificationService* getInstance() {
    if(instance == nullptr) {
        instance = new NotificationService();
    }
    return instance;
}

Usage:

C++

NotificationService* notificationService = NotificationService::getInstance();

This ensures there is only one shared service managing all notifications.

* * *

🚀 How It Works
===============

1. Client creates `NotificationService`
2. Observers attach to `NotificationObservable`
3. Notification is created using Decorators
4. `sendNotification()` is called
5. `NotificationObservable` notifies:
   * Logger (logs notification)
   * NotificationEngine (dispatches via strategies)

System Flow:

Client  
→ NotificationService  
→ NotificationObservable  
→ Observers  
→ Strategies  

* * *

📂 Project Structure
====================

Code

NotificationSystem.cpp  
NotificationSystemUpdated.cpp   (Second version)  
README.md 