# Linked List

연결 리스트는 기초적인 연산을 완벽하게 이해해야 한다. Head Pointer Tracking(머리원소 추적), List Traverse(리스트 종주), Insert/Delete Elements(리스트 원소 추가/제거) 등이 있다.

[Singly linked-list example]

```cc
ListElement<int>* head = nullptr;
insertInFront(&head, 10);

ListElement<int>* current = head;
current->setNext(new ListElement<int>(20));
current = current->getNext();
current->setNext(new ListElement<int>(30));
current = current->getNext();
```

## Class Implementation

```cc
template<typename T>
class ListElement {
public:
    ListElement(const T& value) : next(nullptr), data(value) {}
    ~ListElement() {}

    ListElement* getNext() const { return next; }
    const T& getValue() const { return data; }
    void setNext(ListElement* elem) { next = elem; }
    void setValue(const T& value) { data = value; }

private:
    ListElement* next;
    T data;
};
```

## Head Pointer Tracking

```cc
template<typename T>
bool insertInFront(ListElement<T>** head, const T& value) {
    ListElement<T>* newElem = new ListElement<T>(value);
    if (!newElem) return false;
    *head = newElem;
    return true;
}
```

## List Traverse

```cc
template<typename T>
void print(ListElement<T>* head) {
    ListElement<T>* current = head;
    while (current) {
        std::cout << current->getValue() << std::endl;
        current = current->getNext();
    }
}
```

## Insert/Delete Elements

Not Yet.

[Singly linked-list example]:https://gist.github.com/seungin/632708e3cbde6a70fdbccae54cfbddfa