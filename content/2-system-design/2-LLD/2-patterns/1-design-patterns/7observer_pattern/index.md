


## Publisher and Subscriber

- **Subject (or Publisher):** The object that has the interesting state. It maintains a list of subscribers (Observers) and provides methods to add (`subscribe`) or remove (`unsubscribe`) them. When its state changes, it notifies all its subscribers by calling a specific method on them.
    
- **Observer (or Subscriber):** The object that wants to be notified of the Subject's changes. It must have a specific method (e.g., `update()`) that the Subject can call.