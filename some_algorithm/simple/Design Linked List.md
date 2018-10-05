Question:

Design Linked List
``` js
MyLinkedList = class {
    constructor() {
        this.node = null;
        this.ListNode = class {
            constructor(val) {
                this.val = val;
                this.next = null;
            }
        }
    }

    get(idx) {
        let node = this.node;
        if (!node) return -1;
        let val;
        let i = 0;
        while (i++ <= idx) {
            if (!node) return -1;
            val = node.val;
            node = node.next;
        }
        return val;
    }

    addAtHead(val) {
        if (val == null) throw Error('must assign val as node val');
        if (!this.node) return this.node = new this.ListNode(val);
        const tmp = this.node;
        this.node = new this.ListNode(val);
        this.node.next = tmp;
        return this.node;
    }

    addAtIndex(idx, val) {
        let node= this.node;
        let i = 0;
        while (i++ <= idx) {
            if (!node) return;
            node = node.next;
        }
        if (node) {
            const tmp = node;
            node = new this.ListNode(val);
            node.next = tmp;
        }
    }

    addAtTail(val) {
        let node= this.node;
        if (!node) return;
        while (node && node.next) {
            node = node.next;
        }
        node.next = new this.ListNode(val);
    }

    deleteAtIndex(idx) {
        if (idx === 0) {
            return this.node = this.node.next
        }
        let node = this.node;
        let i = 0;
        let prev = null;
        while (i++ < idx) {
            if (!node) return;
            i === idx && (prev = node);
            node = node.next;
        }
        prev.
    }
}
```