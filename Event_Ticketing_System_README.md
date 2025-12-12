# 🎫 Event Ticketing System (Queue + Stack)

This project simulates an **Event Ticketing System** using two fundamental data structures:

- **Queue** → manages waiting customers in **FIFO** order  
- **Stack** → stores the last 10 served transactions in **LIFO** order  

The system allows users to **add customers, serve them, view the next customer, display the full queue, and check recent transactions**.

---

## 🏗️ System Design

### Queue (Waiting Line)
- Fixed-size array of 20 elements  
- Stores **customer name** + **ticket number**  
- Ticket numbers auto-increment  
- **Operations**:  
  - `enqueue(name)`  
  - `dequeue(ticket)`  
  - `showNext()`  
  - `display()`

### Stack (Last 10 Transactions)
- Fixed-size array of 10  
- Stores **name + ticket + log**  
- Auto-shifts left when full  
- **Operations**:  
  - `push(name, ticket)`  
  - `display()`

---

## 🔄 Program Flow
1. Add Customer  
2. Serve Customer  
3. Show Next Customer  
4. Show Full Queue  
5. Show Last Transactions  
6. Exit  

---

## 🖥️ Sample Run

```text
=== Event Ticketing System ===
1. Add Customer
2. Serve Customer
3. Show Next Customer
4. Show Full Queue
5. Show Last Transactions
6. Exit

Choice: 1
Enter name: John
John joined the queue (Ticket #1).

Choice: 1
Enter name: Mary
Mary joined the queue (Ticket #2).

Choice: 1
Enter name: Alex
Alex joined the queue (Ticket #3).

Choice: 4
Current Queue:
John (Ticket #1)
Mary (Ticket #2)
Alex (Ticket #3)

Choice: 3
Next: John (Ticket #1)

Choice: 2
Serving John (Ticket #1)

Choice: 2
Serving Mary (Ticket #2)

Choice: 5
Recent Transactions:
Served Customer: Mary (Ticket #2)
Served Customer: John (Ticket #1)

Choice: 6
Goodbye!
```

---

## 📦 Source Code

```cpp
#include <iostream>
#include <string>
using namespace std;

class Queue {
private:
    static const int MAX = 20;
    string customers[MAX];
    int tickets[MAX];
    int front, rear;
    int nextTicket;

public:
    Queue() {
        front = -1;
        rear = -1;
        nextTicket = 1;
    }

    bool isEmpty() { return front == -1; }
    bool isFull()  { return rear == MAX - 1; }

    void enqueue(string name) {
        if (isFull()) {
            cout << "Queue is full.\n";
            return;
        }

        if (isEmpty())
            front = 0;

        rear++;
        customers[rear] = name;
        tickets[rear] = nextTicket++;

        cout << name << " joined the queue (Ticket #" << tickets[rear] << ").\n";
    }

    string dequeue(int &ticket) {
        if (isEmpty())
            return "";

        string name = customers[front];
        ticket = tickets[front];

        if (front == rear)
            front = rear = -1;
        else
            front++;

        return name;
    }

    void showNext() {
        if (isEmpty())
            cout << "Queue is empty.\n";
        else
            cout << "Next: " << customers[front]
                 << " (Ticket #" << tickets[front] << ")\n";
    }

    void display() {
        if (isEmpty()) {
            cout << "No customers in queue.\n";
            return;
        }

        cout << "Current Queue:\n";
        for (int i = front; i <= rear; i++)
            cout << customers[i]
                 << " (Ticket #" << tickets[i] << ")\n";
    }
};

class Stack {
private:
    static const int MAX = 10;
    string nameS[MAX];
    int ticketS[MAX];
    string logs[MAX];
    int top;

public:
    Stack() { top = -1; }

    bool isEmpty() { return top == -1; }

    void push(string name, int ticket) {
        string log = "Served Customer";

        if (top == MAX - 1) {
            for (int i = 0; i < MAX - 1; i++) {
                nameS[i] = nameS[i+1];
                ticketS[i] = ticketS[i+1];
                logs[i] = logs[i+1];
            }
            top = MAX - 2;
        }

        top++;
        nameS[top] = name;
        ticketS[top] = ticket;
        logs[top] = log;
    }

    void display() {
        if (isEmpty()) {
            cout << "No recent transactions.\n";
            return;
        }

        cout << "Recent Transactions:\n";

        for (int i = top; i >= 0; i--) {
            cout << logs[i] << ": "
                 << nameS[i] << " (Ticket #"
                 << ticketS[i] << ")\n";
        }
    }
};

int main() {
    Queue q;
    Stack s;
    int choice;

    do {
        cout << "\n=== Event Ticketing System ===\n";
        cout << "1. Add Customer\n";
        cout << "2. Serve Customer\n";
        cout << "3. Show Next Customer\n";
        cout << "4. Show Full Queue\n";
        cout << "5. Show Last Transactions\n";
        cout << "6. Exit\n";
        cout << "Choice: ";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1: {
                string name;
                cout << "Enter name: ";
                getline(cin, name);
                q.enqueue(name);
                break;
            }

            case 2: {
                int ticket;
                string served = q.dequeue(ticket);

                if (served == "")
                    cout << "Queue is empty.\n";
                else {
                    cout << "Serving " << served
                         << " (Ticket #" << ticket << ")\n";

                    s.push(served, ticket);
                }
                break;
            }

            case 3:
                q.showNext();
                break;

            case 4:
                q.display();
                break;

            case 5:
                s.display();
                break;

            case 6:
                cout << "Goodbye!\n";
                break;

            default:
                cout << "Invalid choice.\n";
        }

    } while (choice != 6);

    return 0;
}
```
